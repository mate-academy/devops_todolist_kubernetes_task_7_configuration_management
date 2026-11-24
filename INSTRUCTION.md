# Task 7: Configuration management

## 1. Apply the changes

Apply the manifests in this order (the namespace first, then the ConfigMap and the Secret,
and the Deployment last because it uses them):

```
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

- `configMap.yml` creates the ConfigMap `todoapp-config` with `PYTHONUNBUFFERED`.
- `secret.yml` creates the Secret `todoapp-secret` with `SECRET_KEY`.
- `deployment.yml` reads both through `valueFrom` (`configMapKeyRef` and `secretKeyRef`).
- `src/todolist/settings.py` reads the key from the environment:
  `SECRET_KEY = os.environ.get("SECRET_KEY", "insecure-local-dev-key")`.
  The fallback value is used only for a local run without Kubernetes.

## 2. Validate the changes

Check that the resources exist and the pod is running:

```
kubectl get pods,configmap,secret -n todoapp
```

Expected result: the ConfigMap `todoapp-config`, the Secret `todoapp-secret` and a pod `1/1 Running`.

Check that the variables are set in the container (use the exact pod name from the previous command):

```
kubectl exec <pod-name> -n todoapp -- env | grep -e PYTHONUNBUFFERED -e SECRET_KEY
```

Expected result:

```
PYTHONUNBUFFERED=1
SECRET_KEY=change-me-demo-secret-key-1234567890-abcdefghijklmnopqrstuvwxyz
```

Check the values stored in the resources:

```
kubectl describe configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp -o jsonpath="{.data.SECRET_KEY}"
```

The Secret value is stored base64-encoded (`stringData` in the manifest is encoded by Kubernetes).

## 3. Build and deploy an image with the changed settings.py

The prebuilt image `ikulyk404/todoapp:3.0.0` contains the old `settings.py`, so it does not read
`SECRET_KEY` from the environment. Build and push your own image from the changed code
(replace `<dockerhub-user>` with your Docker Hub login):

```
docker build -t <dockerhub-user>/todoapp:3.1.0 -f src/Dockerfile src
docker push <dockerhub-user>/todoapp:3.1.0
```

Then point the Deployment to the new image and wait for the rollout:

```
kubectl set image deployment/todoapp todoapp=<dockerhub-user>/todoapp:3.1.0 -n todoapp
kubectl rollout status deployment/todoapp -n todoapp
```

Alternatively change `image:` in `.infrastructure/deployment.yml` and run
`kubectl apply -f .infrastructure/deployment.yml`.

`settings.py` has no fallback: `SECRET_KEY = os.environ["SECRET_KEY"]`, so the application
requires the variable from the Secret. For a local run without Kubernetes set it first,
for example `export SECRET_KEY=local-test-key`.

## 4. Notes

The value in `secret.yml` is a demo value. The key that was hardcoded in `settings.py` was
public in Git history, so it must not be reused. In a real project a Secret manifest with a
real key is not committed to Git.
