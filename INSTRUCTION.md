# Task 7: ConfigMap and Secret

## Prerequisites

- `kubectl` configured for your cluster (Minikube, Docker Desktop Kubernetes, kind, etc.)

## Apply manifests

From the repository root, apply namespace and configuration resources first, then the rest of the stack:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
```

Optional (from earlier tasks):

```bash
kubectl apply -f .infrastructure/hpa.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

Or apply everything under `.infrastructure/` after ConfigMap and Secret:

```bash
kubectl apply -f configMap.yml -f secret.yml -f .infrastructure/
```

Wait until the Deployment is available:

```bash
kubectl wait --for=condition=Available deployment/todoapp -n todoapp --timeout=180s
kubectl get pods,configmap,secret -n todoapp
```

## What was configured

| Resource | Name | Purpose |
|----------|------|---------|
| ConfigMap | `todoapp-config` | `PYTHONUNBUFFERED=1` |
| Secret | `todoapp-secret` | Django `SECRET_KEY` (same value as previously hardcoded in `settings.py`, stored base64-encoded in `data`) |
| Deployment | `todoapp` | Injects env vars via `configMapKeyRef` and `secretKeyRef` |

In `src/todolist/settings.py`, `SECRET_KEY` is read from the environment: `os.getenv("SECRET_KEY")`.

## Validate ConfigMap and Secret

Check that objects exist in namespace `todoapp`:

```bash
kubectl get configmap todoapp-config -n todoapp -o yaml
kubectl get secret todoapp-secret -n todoapp -o yaml
```

Decode the secret value locally (should match the former hardcoded key):

```bash
kubectl get secret todoapp-secret -n todoapp -o jsonpath='{.data.SECRET_KEY}' | base64 -d
echo
```

## Validate the Deployment uses them

Pick a running todoapp pod:

```bash
POD=$(kubectl get pods -n todoapp -l app=todoapp -o jsonpath='{.items[0].metadata.name}')
kubectl exec -n todoapp "$POD" -- printenv PYTHONUNBUFFERED
kubectl exec -n todoapp "$POD" -- printenv SECRET_KEY
```

Expected:

- `PYTHONUNBUFFERED` → `1`
- `SECRET_KEY` → non-empty string (the Django secret)

Alternative (describe pod env sources):

```bash
kubectl describe pod -n todoapp "$POD" | sed -n '/Environment:/,/Mounts:/p'
```

You should see references to `todoapp-config` and `todoapp-secret`.

## Validate the application

Health and readiness:

```bash
kubectl port-forward -n todoapp service/todoapp-service 8080:80
```

In another terminal:

```bash
curl -sS http://127.0.0.1:8080/api/health
curl -sS http://127.0.0.1:8080/api/ready
```

If health checks succeed, Django started with `SECRET_KEY` from the Secret (missing or invalid key would prevent normal startup).

Logs:

```bash
kubectl logs -n todoapp -l app=todoapp --tail=30
```

## Clean up (optional)

```bash
kubectl delete -f secret.yml
kubectl delete -f configMap.yml
kubectl delete -f .infrastructure/ --ignore-not-found
```
