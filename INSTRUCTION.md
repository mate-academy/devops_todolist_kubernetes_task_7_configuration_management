### Commands to apply all the changes

```bash
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml 
kubectl apply -f .infrastructure/deployment.yml 
```

### How to validate the changes

```bash
kubectl get secrets -n todoapp
kubectl get configmap -n todoapp
kubectl get secret app-secrets -o jsonpath=’{.data.*}’ -n todoapp
```