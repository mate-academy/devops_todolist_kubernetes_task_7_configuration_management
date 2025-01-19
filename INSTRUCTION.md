# Instructions to Apply Changes

1. **Create ConfigMap and Secret:**
   - Run the following command to create the ConfigMap and Secret:
   
     ```bash
     kubectl apply -f configMap.yml
     kubectl apply -f secret.yml
     ```

2. **Deploy the Updated Application:**
   - Apply the updated deployment:

     ```bash
     kubectl apply -f deployment.yml
     ```

3. **Verify the Changes:**
   - To verify if the environment variables have been applied correctly, check the logs of the pod:

     ```bash
     kubectl logs <pod-name>
     ```

   - Check that the app is using the correct `SECRET_KEY` and `PYTHONUNBUFFERED` values.
