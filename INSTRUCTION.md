### To apply to apply all the changes:
Create or update `configMap.yml`, `secret.yml` and `deployment.yml`, than apply changes:
```bash
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

Check if `configMap.yml`, `secret.yml` and `deployment.yml`, are started:
```bash
kubectl get configmap -n todoapp
kubectl get secrets -n todoapp
kubectl get deployment -n todoapp
```

Deployment need to restart to use changes from `configMap.yml` or `secret.yml`:
```bash
kubectl rollout restart deployment/todoapp -n todoapp
```

### To validate the changes
Get list of pods:
```bash
kubectl get pods -n todoapp
```
Run `env` command within the pod created by deployment:
```bash
kubectl exec -it -n todoapp <pod-name> -- env
```