# Kubernetes Deployment Instructions

## Apply resources

1. Create the ConfigMap:
   ```bash
   kubectl apply -f configMap.yml
   ```
2. Create the Secret:
   ```bash
   kubectl apply -f secret.yml
   ```
3. Create the Deployment:
   ```bash
   kubectl apply -f deployment.yml
   ```

## Validate the changes

1. Confirm the ConfigMap and Secret exist:
   ```bash
   kubectl get configmap todoapp-config
   kubectl get secret todoapp-secret
   ```
2. Confirm the deployment is available:
   ```bash
   kubectl rollout status deployment/todoapp
   kubectl get deployments
   ```
3. Confirm the pod is running:
   ```bash
   kubectl get pods -l app=todoapp
   ```
4. Confirm the environment variables are set in the pod:
   ```bash
   POD_NAME=$(kubectl get pods -l app=todoapp -o jsonpath='{.items[0].metadata.name}')
   kubectl exec -it "$POD_NAME" -- printenv PYTHONUNBUFFERED SECRET_KEY
   ```

## Notes

- `PYTHONUNBUFFERED` is provided by `configMap.yml`.
- `SECRET_KEY` is provided by `secret.yml` and consumed by the Django app via environment variables.
