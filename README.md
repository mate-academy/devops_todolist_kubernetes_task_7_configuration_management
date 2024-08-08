# Deployment Guide
1. **Apply configmap Configuration**
    ```sh
    kubectl apply -f confgiMap.yml
    ```

2. **Apply secret Configuration**
    ```sh
    kubectl apply -f secret.yml
    ```

## Verification

1. **Check Pods**
    ```sh
    kubectl get pods -n todoapp
    ```

2. **Check Logs**
    ```sh
    kubectl logs <name_of_pod> -n todoapp
    ```

Replace `<name_of_pod>` with the actual name of the pod you want to check logs for.
