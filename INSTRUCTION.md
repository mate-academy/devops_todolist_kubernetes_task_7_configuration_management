# ToDo App ConfigMap and Secret Management

## 1. How to apply the changes
To deploy the application with the new ConfigMap and Secret configuration, run:
```bash
# Apply ConfigMap and Secret first
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml

# Apply the Deployment (which uses the updated image version 4.0.0)
kubectl apply -f .infrastructure/deployment.yml
```

## 2. Configuration Details
* **ConfigMap:** Used for non-sensitive data. We store the `PYTHONUNBUFFERED=1` variable here.
* **Secret:** Used for sensitive data. We use the `stringData` field (best practice) to securely inject the Django `SECRET_KEY` into the pod without hardcoding it in the source code or Deployment manifest.

## 3. How to validate the changes

### Check if Resources Exist
```bash
kubectl get configmap todoapp-configmap -n mateapp
kubectl get secret todoapp-secret -n mateapp
```

### Verify Environment Variable Injection
Execute into a running pod and verify that the application successfully pulled the variables from the ConfigMap and Secret:
```bash
# 1. Get the pod name
kubectl get pods -n mateapp -l app=todolist

# 2. Check the environment variables (Replace <pod-name> with the actual name)
kubectl exec -it <pod-name> -n mateapp -- env | grep -E 'PYTHONUNBUFFERED|SECRET_KEY'
```
*Expected Output:* You should see both `PYTHONUNBUFFERED` and `SECRET_KEY` printed in the console with their correct values.

### Verify Application Health
Check that the application is running normally and passing its readiness probes (meaning it successfully used the dynamically injected `SECRET_KEY` instead of a hardcoded one):
```bash
kubectl get pods -n mateapp
```