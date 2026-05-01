# Instructions to Apply Changes

## Apply ConfigMap and Secret
1. Apply the ConfigMap:
   ```bash
   kubectl apply -f .infrastructure/configmap.yml
   ```
2. Apply the Secret:
   ```bash
   kubectl apply -f .infrastructure/secret.yml
   ```

## Apply Deployment
3. Apply the Deployment:
   ```bash
   kubectl apply -f .infrastructure/deployment.yml
   ```

## Validate Changes
1. Verify the ConfigMap is applied:
   ```bash
   kubectl get configmap mateapp-config -n mateapp
   ```
2. Verify the Secret is applied:
   ```bash
   kubectl get secret mateapp-secret -n mateapp
   ```
3. Check the Deployment:
   ```bash
   kubectl get deployment todoapp -n todoapp
   ```
4. Inspect the environment variables in a running pod:
   ```bash
   kubectl exec -it <pod-name> -n todoapp -- env | grep PYTHONUNBUFFERED
   kubectl exec -it <pod-name> -n todoapp -- env | grep SECRET_KEY
   ```

## Additional Validation Steps
1. Ensure the application is running correctly by accessing the service endpoint.
2. Check application logs for any errors related to SECRET_KEY or PYTHONUNBUFFERED:
   ```bash
   kubectl logs <pod-name> -n todoapp
   ```