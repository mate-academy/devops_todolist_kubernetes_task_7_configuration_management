# Create the ConfigMap
kubectl apply -f configMap.yml

# Create the Secret
kubectl apply -f secret.yml

# Deploy the updated application
kubectl apply -f deployment.yml

# Ensure the ConfigMap and Secret have been created
kubectl get configmaps -n todoapp
kubectl get secrets -n todoapp

# Check the environment variables in the deployed pod
kubectl exec -it <pod-name> -n todoapp -- printenv | grep PYTHONUNBUFFERED
kubectl exec -it <pod-name> -n todoapp -- printenv | grep SECRET_KEY

# Verify the application is running correctly
# Access the application via the service URL or NodePort
# Check the logs of the pod to ensure there are no errors related to missing environment variables
kubectl logs <pod-name> -n todoapp

# Confirm the liveness and readiness probes are functioning as expected
kubectl describe pod <pod-name> -n todoapp