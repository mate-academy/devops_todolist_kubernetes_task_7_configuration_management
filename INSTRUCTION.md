# Instructions for applying changes

## Apply manifests
```
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
```
# Validate resources
# Check ConfigMap
```
kubectl get configmap todo-config -o yaml
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
kubectl port-forward deployment/todo-deployment 8000:8000
```
curl http://localhost:8000