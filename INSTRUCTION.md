Applying the changes
Create the ConfigMap:

```bash kubectl apply -f configMap.yml ```

Create the Secret:

```bash kubectl apply -f secret.yml ```

Update the Deployment:

```bash kubectl apply -f deployment.yml ```

Make sure you are using the correct Kubernetes namespace. If needed, add -n todoapp to the commands:

```bash kubectl apply -f configMap.yml -n todoapp kubectl apply -f secret.yml -n todoapp kubectl apply -f deployment.yml -n todoapp ```

Validating the changes
Check that the ConfigMap and Secret have been created:

```bash kubectl get configmap app-config -n todoapp kubectl get secret app-secret -n todoapp ```

Verify that the Deployment has been updated and new pods are running:

```bash kubectl get pods -n todoapp ```

Verify that environment variables are set inside the container:

```bash kubectl exec -n todoapp <pod-name> -- printenv PYTHONUNBUFFERED kubectl exec -n todoapp <pod-name> -- printenv SECRET_KEY ```

Verify the application health:

```bash kubectl get pods -n todoapp ```

Make sure that all pods are in the Running status and show Ready 1/1.