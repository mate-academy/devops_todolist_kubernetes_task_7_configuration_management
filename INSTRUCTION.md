To apply all changes run:
```
# Apply configMap
kubectl apply -f configMap.yml

# Apply secrets
kubectl apply -f secret.yml

# Apply changes to deployment
kubectl apply -f .infrastructure/deployment.yml
```

If all correct when you run
```
kubectl get pods -n todoapp -o wide
```
You should see the pod with Status "Running" and Ready 1/1