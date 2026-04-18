## Apply

```bash
kubectl create namespace todoapp
kubectl apply -f .infrastructure/
```

## Validate

```bash
kubectl get configmap,secret,deployment,pods -n todoapp
kubectl exec -n todoapp deploy/todoapp -- sh -c "env | grep -E 'PYTHONUNBUFFERED|SECRET_KEY'"
```
