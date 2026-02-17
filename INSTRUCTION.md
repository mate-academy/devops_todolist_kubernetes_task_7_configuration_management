# Kubernetes apply and validation instructions

## 1. Apply manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/confgiMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
```

## 2. Validate resources

```bash
kubectl -n todoapp get configmap todoapp-config
kubectl -n todoapp get secret todoapp-secret
kubectl -n todoapp get deployment todoapp
kubectl -n todoapp get pods -l app=todoapp
```

Check that ConfigMap has `PYTHONUNBUFFERED`:

```bash
kubectl -n todoapp get configmap todoapp-config -o yaml
```

Check that Secret has `SECRET_KEY`:

```bash
kubectl -n todoapp get secret todoapp-secret -o jsonpath='{.data.SECRET_KEY}' | base64 -d && echo
```

## 3. Validate env vars inside pod

```bash
kubectl -n todoapp wait --for=condition=Ready pod -l app=todoapp --timeout=120s
POD_NAME=$(kubectl -n todoapp get pod -l app=todoapp --field-selector=status.phase=Running -o jsonpath='{.items[0].metadata.name}')
kubectl -n todoapp exec "$POD_NAME" -- printenv PYTHONUNBUFFERED
kubectl -n todoapp exec "$POD_NAME" -- sh -c 'test -n "$SECRET_KEY" && echo SECRET_KEY_SET'
```

Expected output:
- `PYTHONUNBUFFERED` command returns `1`
- second command returns `SECRET_KEY_SET`

## 4. Validate deployment rollout and app health

```bash
kubectl -n todoapp rollout status deployment/todoapp
kubectl -n todoapp port-forward svc/todoapp-service 8080:80
```

In another terminal:

```bash
curl -i http://127.0.0.1:8080/api/health
curl -i http://127.0.0.1:8080/api/ready
```
