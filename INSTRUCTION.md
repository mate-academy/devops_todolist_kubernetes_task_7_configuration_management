### Apply the changes
```
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
```
### Check ENVs
```
kubectl exec -it <pod_name> -- env
```
