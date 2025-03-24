Instructions for Applying Changes and Validating Them
1. Applying the Changes
Create the ConfigMap:
Run the command to create the ConfigMap from the configMap.yml file:

bash
kubectl apply -f configMap.yml
Create the Secret:
Run the command to create the Secret from the secret.yml file:

bash
kubectl apply -f secret.yml
Update the Deployment:
Run the command to update the Deployment from the deployment.yml file:

bash
kubectl apply -f deployment.yml
2. Validating the Changes
Validate the ConfigMap
Ensure the ConfigMap is created:

bash
kubectl get configmap todoapp-config -n todoapp
View the contents of the ConfigMap:

bash
kubectl describe configmap todoapp-config -n todoapp
Validate the Secret
Ensure the Secret is created:

bash
kubectl get secret todoapp-secret -n todoapp
View the contents of the Secret (values will be base64 encoded):

bash
kubectl describe secret todoapp-secret -n todoapp
Validate the Deployment
Ensure the Deployment is updated:

bash
kubectl get deployment todoapp -n todoapp
View the details of the Deployment to confirm the use of ConfigMap and Secret:

bash
kubectl describe deployment todoapp -n todoapp
Verify that the environment variables are correctly set in the container:

Get the name of the Pod belonging to the Deployment:

bash
kubectl get pods -n todoapp -l app=todoapp
Check the environment variables in the container:

bash
kubectl exec <pod-name> -n todoapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
Replace <pod-name> with the name of your Pod. You should see the values of PYTHONUNBUFFERED and SECRET_KEY.

3. Verify Application Functionality
Ensure the application is running correctly:

Get the URL or IP address of the service:

bash
kubectl get svc -n todoapp
Make a request to the application to verify its availability (e.g., using curl or a browser).

Ensure the application uses the SECRET_KEY from the Secret and not a hardcoded value:

Check the application logs:

bash
kubectl logs <pod-name> -n todoapp
Confirm that the application does not return errors related to SECRET_KEY.

4. Conclusion
After executing these commands and performing the checks, you should see that:

The ConfigMap and Secret are successfully created and used by the Deployment.

The application runs without errors and uses the correct environment variables.

