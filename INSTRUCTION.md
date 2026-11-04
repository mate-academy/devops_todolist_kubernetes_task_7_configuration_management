# Instructions for Applying and Validating Kubernetes Configuration

This guide contains the commands to deploy the updated configuration resources and instructions to validate that the changes were applied correctly.

---

## 1. Apply Changes to the Cluster

Run the following commands in your terminal to apply the updated manifests in the correct order (ConfigMap and Secret must be created before the Deployment uses them).

```bash
# 1. Apply the ConfigMap
kubectl apply -f configMap.yml

# 2. Apply the Secret
kubectl apply -f secret.yml

# 3. Apply (or restart) the Deployment
kubectl apply -f deployment.yml
```

*Note: If the deployment was already running, you can force a rolling update to pick up the new ConfigMap/Secret values immediately:*
```bash
kubectl rollout restart deployment/todoapp -n todoapp
```

---

## 2. Validate the Changes

Follow these steps to ensure that your application successfully received the variables and is running properly.

### Step 2.1: Check Pod Status
Ensure that the pods are running and that the `livenessProbe` and `readinessProbe` are passing (which confirms the paths `/api/health` and `/api/ready` are correct).
```bash
kubectl get pods -n todoapp
```
*Expected output: Status should be `Running` and `READY` should show `1/1` after a few moments.*

### Step 2.2: Verify Environment Variables inside the Container
To make sure `PYTHONUNBUFFERED` and `SECRET_KEY` are mounted correctly from the resources, execute a command inside the running pod to print its environment variables.

1. Get the exact name of your running pod:
   ```bash
   kubectl get pods -n todoapp
   ```
2. Run `env` inside that pod (replace `<pod-name>` with your actual pod name):
   ```bash
   kubectl exec -it <pod-name> -n todoapp -- env
   ```

*Expected output: In the list of variables, you should see exactly:*
* `PYTHONUNBUFFERED=1`
* `SECRET_KEY=29I30BppcHrDAy8q_L7VAoAK27RjsyfnvqlVd03C-r8JOrMw1Fon0G5Q6LCxYGH7br8`

### Step 2.3: Check Application Logs
Check the logs of the container to ensure Django started up without any configuration or `KeyError` issues.
```bash
kubectl logs deployment/todoapp -n todoapp
```
*Expected output: Standard Django startup logs (e.g., "Starting development server at http://0.0.0") without any Python tracebacks related to `settings.py`.*
