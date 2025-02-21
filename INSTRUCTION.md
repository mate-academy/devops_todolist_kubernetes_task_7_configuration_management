# Implementing secret and configMap into TodoApp

## 1. Apply updated manifests:
```bash 
 kubectl apply -f ./.infrascructure/configMap.yml

 kubectl apply -f ./.infrascructure/secret.yml

 kubectl apply -f ./.infrascructure/deployment.yml
```
## 2. Check the status of the pods and pod info:
```bash
 kubectl get pods -n todoapp

kubectl get secrets -n todoapp

kubectl get configmap -n todoapp
``` 
## 3. Check new environment and secret values:
```bash
 kubectl exec -it <pod-name> -n todoapp -- printenv | grep PYTHONUNBUFFERED 
kubectl exec -it <pod-name> -n todoapp -- printenv | grep SECRET_KEY
```