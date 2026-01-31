```bash
# Apply configMap
kubectl apply -f .infrastructure/configMap.yml

# Apply secret
kubectl apply -f .infrastructure/secret.yml

# Apply deployment with configMap and secret usage
kubectl apply -f .infrastructure/deployment.yml

# Get names of pods
kubectl get pods -n todoapp

# Connect to pod and print all env variables
kubectl exec -it -n todoapp <pod_name> -- printenv
```