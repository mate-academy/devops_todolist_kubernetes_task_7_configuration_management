To apply the changes, use the following commands:

1. Create the ConfigMap:
    ```sh
    kubectl apply -f configMap.yml
    ```

2. Create the Secret:
    ```sh
    kubectl apply -f secret.yml
    ```

3. Deploy the updated application:
    ```sh
    kubectl apply -f deployment.yml
    ```

To validate the changes, follow these steps:

1. Ensure the ConfigMap and Secret have been created:
    ```sh
    kubectl get configmaps -n todoapp
    kubectl get secrets -n todoapp
    ```

2. Check the environment variables in the deployed pod:
    ```sh
    kubectl exec -it <pod-name> -n todoapp -- printenv | grep PYTHONUNBUFFERED
    kubectl exec -it <pod-name> -n todoapp -- printenv | grep SECRET_KEY
    ```

3. Verify the application is running correctly:
    - Access the application via the service URL or NodePort.
    - Check the logs of the pod to ensure there are no errors related to missing environment variables:
      ```sh
      kubectl logs <pod-name> -n todoapp
      ```

4. Confirm the liveness and readiness probes are functioning as expected:
    ```sh
    kubectl describe pod <pod-name> -n todoapp
    ```