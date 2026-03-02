# Kubernetes configuration management: apply and validation

## 1. Apply resources

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/confgiMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

## 2. Validate ConfigMap and Secret

```bash
kubectl get configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp
```

## 3. Validate Deployment wiring

```bash
kubectl describe deployment todoapp -n todoapp
```

Check that:
- `PYTHONUNBUFFERED` is loaded from ConfigMap `todoapp-config`
- `SECRET_KEY` is loaded from Secret `todoapp-secret`

## 4. Validate runtime environment in a pod

```bash
POD_NAME=$(kubectl get pods -n todoapp -l app=todoapp -o jsonpath='{.items[0].metadata.name}')
kubectl exec -n todoapp "$POD_NAME" -- printenv PYTHONUNBUFFERED
kubectl exec -n todoapp "$POD_NAME" -- printenv SECRET_KEY
```

Expected:
- `PYTHONUNBUFFERED` prints `1`
- `SECRET_KEY` prints non-empty value from `secret.yml`

## 5. Optional health check

```bash
kubectl port-forward -n todoapp deployment/todoapp 8080:8080
curl -i http://localhost:8080/api/health
curl -i http://localhost:8080/api/ready
```
