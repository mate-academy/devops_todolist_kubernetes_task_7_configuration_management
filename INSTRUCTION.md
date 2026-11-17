# Django ToDo — Kubernetes Configuration Management

## 1. Prerequisites

Before deploying the application, make sure that:

- Kubernetes is running in Docker Desktop.
- `kubectl` is connected to the cluster.
- Docker is installed and configured.
- The application image `aniapavliuk97/todoapp:4.0.0` is available on Docker Hub.

All commands should be executed from the root directory of the repository.

## 2. Configuration Resources

The application uses two Kubernetes configuration resources.

**ConfigMap: `todo-config`**

The ConfigMap stores the `PYTHONUNBUFFERED` environment variable with the value `"1"`. It is referenced in the Deployment using `configMapKeyRef`.

**Secret: `todo-secret`**

The Secret stores the `SECRET_KEY` value required by Django. It is referenced in the Deployment using `secretKeyRef`.

The application reads the key from an environment variable in `settings.py`:

```python
SECRET_KEY = os.environ["SECRET_KEY"]
```

The original hardcoded value is no longer used.

The Secret manifest can use `data` with Base64-encoded values or `stringData` with plain-text input.

**Security note:** Base64 is encoding, not encryption. Do not commit real credentials to GitHub. Use a placeholder in the committed manifest and provide a fresh key locally when deploying.

## 3. Building the Docker Image

The application code was updated so that Django reads `SECRET_KEY` from the environment.

Build the Docker image:

```bash
docker build -t aniapavliuk97/todoapp:4.0.0 ./src
```

Push it to Docker Hub:

```bash
docker push aniapavliuk97/todoapp:4.0.0
```

The Dockerfile uses a temporary, non-sensitive value for build-time migrations because Kubernetes Secrets are not available during image building.

The actual application key is supplied at runtime through Kubernetes Secret.

## 4. Deploying Kubernetes Resources

First, create or update the namespace:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Validate the manifests:

```bash
kubectl apply --dry-run=server -f .infrastructure/configMap.yml
kubectl apply --dry-run=server -f .infrastructure/secret.yml
kubectl apply --dry-run=server -f .infrastructure/deployment.yml
```

Deploy the ConfigMap:

```bash
kubectl apply -f .infrastructure/configMap.yml
```

Deploy the Secret:

```bash
kubectl apply -f .infrastructure/secret.yml
```

Deploy the application:

```bash
kubectl apply -f .infrastructure/deployment.yml
```

Verify that the Deployment was successfully updated:

```bash
kubectl rollout status deployment/todoapp -n todoapp
```

Check the Pods:

```bash
kubectl get pods -n todoapp -l app=todoapp
```

The application Pods should reach `Running` and `1/1` readiness.

## 5. Validating ConfigMap and Secret

Verify that both configuration resources exist:

```bash
kubectl get configmap todo-config -n todoapp
kubectl get secret todo-secret -n todoapp
```

Check that the running container receives the ConfigMap value and that the secret environment variable is present:

```bash
kubectl exec -n todoapp deployment/todoapp -- python -c 'import os; print("PYTHONUNBUFFERED:", os.getenv("PYTHONUNBUFFERED")); print("SECRET_KEY present:", bool(os.getenv("SECRET_KEY")))'
```

Expected output:

```text
PYTHONUNBUFFERED: 1
SECRET_KEY present: True
```

Verify that Django uses the environment variable as its actual secret key:

```bash
kubectl exec -n todoapp deployment/todoapp -- python manage.py shell -c 'import os; from django.conf import settings; print("Django uses injected SECRET_KEY:", settings.SECRET_KEY == os.environ["SECRET_KEY"])'
```

Expected output:

```text
Django uses injected SECRET_KEY: True
```

These commands validate the application configuration without displaying the secret value.

## 6. Accessing the Application

Forward a local port to the Deployment:

```bash
kubectl port-forward -n todoapp deployment/todoapp 8086:8080
```

Open the application in a browser:

http://localhost:8086/

The API is available at:

http://localhost:8086/api/

The health and readiness endpoints can be checked at:

http://localhost:8086/api/health

http://localhost:8086/api/ready

The container listens on port 8080. The default Django port of 8000 in the original README applies when the development server is started without explicitly specifying another port.

Stop port forwarding using `Ctrl+C`.

## 7. Troubleshooting

Check Deployment status:

```bash
kubectl get deployment todoapp -n todoapp
```

Inspect the Deployment:

```bash
kubectl describe deployment todoapp -n todoapp
```

Check application logs:

```bash
kubectl logs -n todoapp deployment/todoapp --tail=50
```

If a Pod cannot start, verify that the ConfigMap and Secret exist in the same namespace as the Deployment and that the referenced names and keys match.

If the Docker build fails with `KeyError: 'SECRET_KEY'`, check whether Django management commands run during the build. A temporary build-only key may be required for migrations. The actual key must not be embedded in the Docker image.

If a ConfigMap or Secret value is changed, running containers do not automatically receive updated environment-variable values.

Apply the updated resource and restart the Deployment:

```bash
kubectl rollout restart deployment/todoapp -n todoapp
```

Verify the rollout:

```bash
kubectl rollout status deployment/todoapp -n todoapp
```

## 8. Submission

Before committing the changes, verify that no real secret keys or credentials are included in the repository.

Check the changes:

```bash
git status
git diff --cached
```

Commit the Kubernetes manifests, updated Django settings, and `INSTRUCTION.md`.

Push the changes to the working branch and create a Pull Request for validation in Mate Academy.