# ToDo App Kubernetes Deployment Instructions

## 1. Apply the changes
To deploy the application and its configurations to the Kubernetes cluster, execute the following commands in your terminal:

```bash
# Create the namespace if it doesn't exist
kubectl create namespace todoapp --dry-run=client -o yaml | kubectl apply -f -

# Apply ConfigMap and Secret
kubectl apply -f configMap.yml
kubectl apply -f secret.yml

# Apply the Deployment
kubectl apply -f deployment.yml