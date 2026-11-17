# Django ToDo — Kubernetes Configuration Management

This guide explains how to deploy the Django ToDo application using a Kubernetes ConfigMap and Secret, and how to verify that the application reads its configuration from environment variables.

## 1. Prerequisites

- A running Kubernetes cluster (the exercises use Docker Desktop).
- `kubectl` connected to the correct cluster.
- Docker and access to the image `aniapavliuk97/todoapp:4.0.0`.
- Run the commands below from the repository root.

The resources used in this task are:

| Resource | Name | Namespace | Purpose |
|---|---|---|---|
| ConfigMap | `todo-config` | `todoapp` | Supplies `PYTHONUNBUFFERED` |
| Secret | `todo-secret` | `todoapp` | Supplies `SECRET_KEY` |
| Deployment | `todoapp` | `todoapp` | Runs the Django application |

## 2. Configuration

The ConfigMap is defined in `.infrastructure/configMap.yml` and contains `PYTHONUNBUFFERED: "1"`. The Deployment reads it using `env[].valueFrom.configMapKeyRef`.

The Secret is defined in `.infrastructure/secret.yml`. Its `SECRET_KEY` entry is referenced by the Deployment using `env[].valueFrom.secretKeyRef`. The Secret can use either `data` (Base64-encoded value) or `stringData` (plain-text input that Kubernetes converts to Secret data).

**Security:** Do not commit a real Django secret key to Git, even in Base64. The version-controlled `.infrastructure/secret.yml` is an **example/template only** and must contain a non-sensitive placeholder. During deployment, generate a fresh key and apply the resulting Secret manifest directly to Kubernetes (see section 3). Do **not** apply the placeholder file to the cluster. The generated key will not be written to the repository or printed. Replace any key that was previously shared.

In `src/todolist/settings.py`, Django must read the value from the environment instead of a hardcoded string:

```python
SECRET_KEY = os.environ["SECRET_KEY"]
```

The `os` module must be imported. `BASE_DIR` is calculated by Django code and does not need to be supplied as a Kubernetes Secret.

The Deployment uses the rebuilt image `aniapavliuk97/todoapp:4.0.0`, which contains the modified `settings.py`. Changing `settings.py` locally does not change an existing Docker image. If rebuilding this repository, use its `src` directory as the build context:

```bash
docker build -t aniapavliuk97/todoapp:4.0.0 ./src
docker push aniapavliuk97/todoapp:4.0.0
```

If the Dockerfile runs `python manage.py migrate` during the build, provide a **non-secret, build-only** placeholder for that one command (for example, `RUN SECRET_KEY=temporary-build-only-key python manage.py migrate`). Do not bake the real key into the image. In a production deployment, migrations should be managed separately.

## 3. Validate and deploy the manifests

Create or confirm the namespace first:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Check the ConfigMap and Deployment manifests without changing the cluster:

```bash
kubectl apply --dry-run=server -f .infrastructure/configMap.yml
kubectl apply --dry-run=server -f .infrastructure/deployment.yml
```

The committed `.infrastructure/secret.yml` contains a **placeholder** and serves only as a template showing the Secret structure. **Do not deploy it as-is.** Instead, generate a new, random Django secret key and supply it to a Kubernetes Secret manifest **before applying the Secret**:

```bash
kubectl create secret generic todo-secret \
  --namespace todoapp \
  --from-literal="SECRET_KEY=$(python3 -c 'import secrets; print(secrets.token_urlsafe(48))')" \
  --dry-run=client -o yaml | kubectl apply -f -
```

This command works in Bash or Zsh. `python3` generates a new key; `kubectl create secret --dry-run=client -o yaml` builds the Secret manifest with that key; and `kubectl apply -f -` applies **that generated manifest**, not the committed placeholder. Nothing is written to `secret.yml`, and the key is not printed. The deployed Secret is named `todo-secret` in the `todoapp` namespace, matching the Deployment's `secretKeyRef`.

**Important:** Never run `kubectl apply -f .infrastructure/secret.yml` while it still contains a placeholder. Doing so would replace the active key with the placeholder.

Now deploy the ConfigMap and application:

```bash
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/deployment.yml
```

If the Deployment was **already running** with the previous Secret value, restart its Pods so they receive the newly generated key:

```bash
kubectl rollout restart deployment/todoapp -n todoapp
```

Wait for the Deployment rollout:

```bash
kubectl rollout status deployment/todoapp -n todoapp
kubectl get pods -n todoapp -l app=todoapp
```

The application Pods should reach `Running` and `1/1` readiness. Confirm the deployed image:

```bash
kubectl get deployment todoapp -n todoapp -o jsonpath='{.spec.template.spec.containers[0].image}'
```

Expected image: `aniapavliuk97/todoapp:4.0.0`.

**Note:** The old standalone ToDo Pods, if any, are not managed by this Deployment and should not be confused with the Pods created by it.

## 4. Validate ConfigMap and Secret integration

Check that the Kubernetes objects exist, without revealing the Secret value:

```bash
kubectl get configmap todo-config -n todoapp
kubectl get secret todo-secret -n todoapp
```

Verify that the container received the ConfigMap value and that the Secret variable exists (the key's value is not printed):

```bash
kubectl exec -n todoapp deployment/todoapp -- python -c 'import os; print("PYTHONUNBUFFERED:", os.getenv("PYTHONUNBUFFERED")); print("SECRET_KEY present:", bool(os.getenv("SECRET_KEY")))'
```

Expected output:

```text
PYTHONUNBUFFERED: 1
SECRET_KEY present: True
```

Verify that Django actually uses the injected key, again without printing it:

```bash
kubectl exec -n todoapp deployment/todoapp -- python manage.py shell -c 'import os; from django.conf import settings; print("Django uses injected SECRET_KEY:", settings.SECRET_KEY == os.environ["SECRET_KEY"])'
```

Expected output:

```text
Django uses injected SECRET_KEY: True
```

These runtime checks matter: successful YAML validation alone does not prove that Django uses the Secret.

## 5. Access the application

Forward a local port to the Deployment:

```bash
kubectl port-forward -n todoapp deployment/todoapp 8086:8080
```

Keep this command running, then open:

- Application: http://localhost:8086/
- API: http://localhost:8086/api/
- Health endpoint: http://localhost:8086/api/health
- Readiness endpoint: http://localhost:8086/api/ready

The container listens on port **8080**, as configured in this Deployment and its Docker image. The default port 8000 in the original README applies to running Django without an explicit port argument.

You can also test the HTTP response from a second terminal:

```bash
curl -i http://localhost:8086/
```

Stop port forwarding with `Ctrl+C`.

## 6. Troubleshooting

Check the Deployment and its events:

```bash
kubectl get deployment todoapp -n todoapp
kubectl describe deployment todoapp -n todoapp
kubectl get pods -n todoapp -l app=todoapp
kubectl logs -n todoapp deployment/todoapp --tail=50
```

If a Pod reports a missing ConfigMap or Secret key, confirm that `todo-config` and `todo-secret` exist in **the same namespace** as the Deployment and that the names and keys exactly match the `valueFrom` references.

If the Docker build fails with `KeyError: 'SECRET_KEY'`, inspect any build-time `manage.py` commands. A Kubernetes Secret is injected only when the container starts in Kubernetes, not during `docker build`.

If you change the Secret or ConfigMap value, existing environment variables in running containers **do not update automatically**. Restart the Deployment after applying the updated resource:

```bash
kubectl rollout restart deployment/todoapp -n todoapp
kubectl rollout status deployment/todoapp -n todoapp
```

Finally, before creating a Pull Request, review the staged files and ensure that no real or previously disclosed secret is included:

```bash
git status
git diff --cached
```

Commit the manifests, updated application settings, and this instruction file, but not real credentials.
