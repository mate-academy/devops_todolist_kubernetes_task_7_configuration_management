### Apply the changes
```
kubectl apply -f confgiMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
```
### Check ENVs
```
kubectl exec -it <pod_name> -- env
```
