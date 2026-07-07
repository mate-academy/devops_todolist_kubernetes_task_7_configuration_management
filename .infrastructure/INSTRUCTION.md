# Deployment Instructions

This document outlines the steps to apply the Kubernetes configuration changes and validate the deployment of the ToDo application.

## Prerequisites

- Access to a Kubernetes cluster.
- `kubectl` configured to interact with your cluster.
- Set your current context namespace to `todoapp`:
  ```bash
  kubectl config set-context --current --namespace=todoapp
  ```

## Apply Changes

1. **Apply the ConfigMap**
   ```bash
   kubectl apply -f .infrastructure/configmap.yml
   ```

2. **Apply the Secret**
   ```bash
   kubectl apply -f .infrastructure/secret.yml
   ```

3. **Apply the Deployment**
   ```bash
   kubectl apply -f .infrastructure/deployment.yml
   ```

## Validation

1. **Verify Resources**
   Ensure the resources were created successfully:
   ```bash
   kubectl get configmap
   kubectl get secret
   kubectl get deployment todoapp
   ```

2. **Verify Environment Variables**
   Check that the Deployment is correctly using the ConfigMap and Secret by inspecting the pod's environment variables:
   ```bash
   kubectl describe deployment todoapp
   ```
   Look for the `Environment:` section to confirm `PYTHONUNBUFFERED` and `SECRET_KEY` are sourced from `config-map` and `app-secret` respectively.

   You can also verify the Secret's value (encoded):
   ```bash
   kubectl get secret app-secret -o jsonpath='{.data.*}'
   ```

3. **Verify Pod Status**
   Check the status of the pods to ensure the application is running:
   ```bash
   kubectl get pods
   ```

4. **Verify Logs**
   Check the logs of the pod to ensure no errors are occurring during startup:
   ```bash
   # Replace <pod-name> with the actual pod name from the previous command
   kubectl logs <pod-name>
   ```
