1. **Apply the Secret:**
   ```
   kubectl apply -f .infrastructure/secret.yml
   ```

2. **Apply the ConfigMap:**
   ```
   kubectl apply -f .infrastructure/confgiMap.yml
   ```

3. **Apply the Deployment:**
   ```
   kubectl apply -f .infrastructure/deployment.yml
   ```

## Validation Steps

1. **Check environment injection:**
   ```
   kubectl get pods -n todoapp
   kubectl describe pod <pod-name> -n todoapp
   ```
   - Ensure `SECRET_KEY` and `PYTHONUNBUFFERED` are present in the env section.

2. **Confirm app readiness:**
   ```
   kubectl get pods -n todoapp
   kubectl logs <pod-name> -n todoapp
   ```
   - Check for absence of SECRET_KEY errors and readiness probe success.