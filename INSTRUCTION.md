1. To apply all the changes, run the following commands in your terminal:

```bash
# Apply the ConfigMap
kubectl apply -f .infrastructure/configMap.yml
# Apply the Secret
kubectl apply -f .infrastructure/secret.yml
# Apply the Deployment
kubectl apply -f .infrastructure/deployment.yml
```

2. To validate the changes, you can check the following:

- Verify that the ConfigMap has been created:

```bash
kubectl get configmap todolist-configmap -n todoapp
kubectl describe configmap todolist-configmap -n todoapp
``` 

- Verify that the Secret has been created:

```bash
kubectl get secret todolist-secret -n todoapp
kubectl describe secret todolist-secret -n todoapp
```