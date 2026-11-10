# commands to apply all the changes
cd /path/to/project/.infrastructure
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml

# instructions on how to validate the changes
kubectl get configmaps -n todoapp
kubectl get configmaps -n todoapp todoapp-config  -o jsonpath=’{.data.PYTHONUNBUFFERED}’
kubectl get secrets -n todoapp
kubectl get secrets -n todoapp todoapp-secrets -o jsonpath=’{.data.SECRET_KEY}
