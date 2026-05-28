# Instructions for delpoying ConfigMap, Secret, and Deployment in Kubernetes
This goide contains information on how to deploy configuration resources('ConfigMap', 'Secret'), update 'Deployment' to use them and check that the changes are applied correctly.
---
## Applying configuration files
To create resources and update the deployment, run the following commands in the terminal:
```bash
# Create a ConfigMap with Python environment variables:
kubectl apply -f configMap.yml

# Create a Secret with the application's secret key:
kubectl apply -f secret.yml

# Update a Deployment using three resources
kubectl apply -f deployment.yml
```

## 2. Validation and verification of changes
To verify that Deployment has successfully applied the settings from the ConfigMap and Secret, complete the following steps:

1. Check the status of the flow:
Make sure that the old pods have been deleted and the new ones have started successfullyand are in the status Running:
```bash
kubectl get pods
```
2. Checking environment variables inside the container
Select the name of the running pods and run the env commandto check the presence and correctness of the PYTHONUNBUFFERED and SECRET_KEY variables:
```bash
kubectl exec -it <pod-name> -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```
