# Instructions for applying changes

## Apply manifests
```
kubectl apply -f ConfigMap.yml -n todoapp
kubectl apply -f secret.yml -n todoapp
kubectl apply -f deployment.yml -n todoapp
```
# Validate resources
# Check ConfigMap
```
kubectl get ConfigMap todo-config -o yaml
```
# Check Secret (encoded value)
```
kubectl get secret todo-secret -o yaml
```
# Decode secret to verify
```
kubectl get secret todo-secret -o jsonpath='{.data.SECRET_KEY}' | base64 --decode
```
# Check Deployment envs
```
kubectl describe pod <pod-name> | grep -A2 "Environment"
```
Test app
```
kubectl port-forward deployment/todoapp 8080:8080 -n todoapp
```
curl http://localhost:8000