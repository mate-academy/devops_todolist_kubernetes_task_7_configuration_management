# Instructions to Apply and Validate Kubernetes Resources

This guide provides steps to deploy the ConfigMap, Secret, and the updated Deployment for the ToDo application.

## 1. Apply Resources
Run the following commands in your terminal to create or update the resources:

```bash
# Create the namespace if it doesn't exist
kubectl create namespace mateapp --dry-run=client -o yaml | kubectl apply -f -

# Apply ConfigMap
kubectl apply -f .infrastructure/configMap.yml

# Apply Secret
kubectl apply -f .infrastructure/secret.yml

# Apply Deployment
kubectl apply -f .infrastructure/deployment.yml
```

## 2. Validation
To verify that everything is working correctly, follow these steps:
Check Resources Status
```bash
kubectl get all -n mateapp
```

All pods should be in Running status.

Connect to one of the pods from deployment and make the next command:
```bash
kubectl exec -it <POD_NAME> -- sh -c "printenv | grep SECRET_KEY"
```

