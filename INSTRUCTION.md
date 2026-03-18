## Kubernetes task 7 — Configuration management

### Apply manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/confgiMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
```

Verify:

```bash
kubectl -n todoapp get configmap todoapp-config -o yaml
kubectl -n todoapp get secret todoapp-secret -o yaml
kubectl -n todoapp get deploy,pods,svc
```

### Validate environment variables inside the pod
Get a pod name:

```bash
kubectl -n todoapp get pods
```

Then check env values:

```bash
kubectl -n todoapp exec -it pod/<POD_NAME> -- printenv | grep -E 'PYTHONUNBUFFERED|SECRET_KEY'
```

Expected:
- `PYTHONUNBUFFERED=1` comes from ConfigMap
- `SECRET_KEY` comes from Secret

### Validate application endpoints

```bash
kubectl -n todoapp port-forward svc/todoapp-service 8080:80
```

In another terminal:

```bash
curl -i http://localhost:8080/api/ready
curl -i http://localhost:8080/api/health
```

