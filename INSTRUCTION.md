# Instructions to Apply Changes

Follow these steps to ensure the Kubernetes resources and application are configured correctly.

## Steps

1. **Apply the ConfigMap**  
   Apply the ConfigMap resource to set the `PYTHONUNBUFFERED` environment variable:

   ```bash
   kubectl apply -f configMap.yml
   ```

2. **Apply the Secret**  
   Apply the Secret resource to store the `SECRET_KEY`:

   ```bash
   kubectl apply -f secret.yml
   ```

3. **Apply the Deployment**  
   Deploy or update the application Deployment to use both the ConfigMap and the Secret:

   ```bash
   kubectl apply -f deployment.yml
   ```

4. **Verify the Secret is Created**  
   Make sure the Secret has been applied successfully:

   ```bash
   kubectl get secret app-secrets -o yaml
   ```
   Check that the `SECRET_KEY` exists in the output (it will be base64 encoded).

5. **Verify the Pod Environment Variables**  
   Ensure that the `SECRET_KEY` is being passed to the application's container:

   - Find the Pod name:

     ```bash
     kubectl get pods
     ```

   - Check the `SECRET_KEY` inside the container's environment variables:

     ```bash
     kubectl exec -it <pod-name> -- printenv | grep SECRET_KEY
     ```

6. **Test the Application**  
   Access the application and test its functionality to ensure it's working as expected with the new configuration.

## Validation  
To validate all changes:

- Ensure no hardcoded `SECRET_KEY` remains in the code (e.g., `settings.py`).
- Confirm the application is retrieving the `SECRET_KEY` dynamically from the environment, which is backed by the Kubernetes Secret.
- Test the app to confirm it behaves as expected.
