# Deployment Instructions & Validation Guide

This document contains step-by-step instructions on how to apply the configuration changes for the `todoapp` and validate that the environment variables from the ConfigMap and Secret are correctly injected.

---


## 1. Applying Configuration
Apply the Kubernetes manifests in the following order (Secret and ConfigMap first, then the Deployment):

```
1. Create the ConfigMap resource
kubectl apply -f configMap.yml

2. Create the Secret resource
kubectl apply -f secret.yml

3. Apply/Update the Deployment
kubectl apply -f deployment.yml
```
## 2. Validation
To ensure everything is deployed correctly and the Django application successfully consumes the environment variables, perform the following validation steps:

### Step 2.1: Verify Resources are Created
Check that the ConfigMap, Secret, and Pods are up and running in the mateapp namespace:

```
kubectl get configmap app-config-todoapp -n mateapp
kubectl get secret app-secret-todoapp -n mateapp
kubectl get pods -n mateapp -l app=todoapp
```
The pods should have a status of Running.

### Step 2.2: Verify Environment Variables inside the Container
Choose one of the running pod names from the previous command output and execution env command inside it to check the injected variables:


 Replace <pod-name> with your actual pod ID (e.g., todoapp-68cb5b68bd-bjc82)
```
kubectl exec -it <pod-name> -n mateapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```
Expected Output:

Plaintext

PYTHONUNBUFFERED=1
SECRET_KEY=@e2(yx)v&tgh3_s=0yja-i!dcpebxsz^dg47x)-k&kq_3zf*9e=

Note: Notice that Kubernetes automatically decoded the base64 string from secret.yml, so the application sees the plain-text Django secret key.
