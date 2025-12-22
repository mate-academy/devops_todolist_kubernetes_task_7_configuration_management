# Configuration Management Instructions

## 1. Apply Changes

To apply the configuration changes, we first need to create the ConfigMap and Secret, and then update the Deployment to restart the pods with the new environment variables.

1.  **Apply ConfigMap and Secret:**
    ```bash
    kubectl apply -f ./infrastructure/configMap.yml
    kubectl apply -f ./infrastructure/secret.yml
    ```

2.  **Apply the updated Deployment:**
    ```bash
    kubectl apply -f ./infrastructure/deployment.yml
    ```
    *This triggers a rolling update. Kubernetes will terminate old pods and create new ones that have the new configuration attached.*

---

## 2. Validation

We need to verify that the environment variables (`PYTHONUNBUFFERED` and `SECRET_KEY`) are correctly injected into the running containers.

1.  **Wait for the new pods to be ready:**
    ```bash
    kubectl get pods -n mateapp -w
    ```
    Wait until you see the new pods in `Running` state and the old ones `Terminating`.

2.  **Check Environment Variables in a Pod:**
    Get the name of one running pod:
    ```bash
    kubectl get pods -n mateapp
    ```
    Execute the `printenv` command inside the pod to list all environment variables:
    ```bash
    kubectl exec <pod-name> -n mateapp -- printenv | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
    ```
    *(Replace `<pod-name>` with the actual name, e.g., `todoapp-deployment-xyz-123`)*

    **Expected Output:**
    You should see something like:
    ```text
    PYTHONUNBUFFERED=1
    SECRET_KEY=my-super-secret-django-key-change-me
    ```

3.  **Validate Application Logic (Optional):**
    If the application logs startup configuration, you can check the logs to ensure it picked up the secret:
    ```bash
    kubectl logs <pod-name> -n mateapp
    ```