# Kubernetes Deployment Instructions

## Prerequisites

- A running Kubernetes cluster (e.g. Minikube, Kind, or a cloud provider)
- `kubectl` installed and configured to talk to your cluster

---

## 1. Set Your Django SECRET_KEY

Before applying the secret, encode your Django `SECRET_KEY` in Base64:

```bash
echo -n 'your-actual-django-secret-key' | base64
```

Replace the `SECRET_KEY` value in `secret.yml` with the output of the above command.

---

## 2. Apply All Manifests

Apply the resources in order (namespace first, then everything else):

```bash
kubectl apply -f namespace.yml
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
kubectl apply -f clusterIp.yml
kubectl apply -f nodeport.yml
kubectl apply -f hpa.yml
kubectl apply -f cronjob.yml
kubectl apply -f daemonset.yml
```

Or apply them all at once if they are in the same directory:

```bash
kubectl apply -f .
```

---

## 3. Validate the Changes

### Check that the namespace exists
```bash
kubectl get namespace todoapp
```

### Verify the ConfigMap was created
```bash
kubectl get configmap todoapp-config -n todoapp
kubectl describe configmap todoapp-config -n todoapp
```

### Verify the Secret was created
```bash
kubectl get secret todoapp-secret -n todoapp
kubectl describe secret todoapp-secret -n todoapp
```

> **Note:** `describe` will not reveal secret values — this is expected behaviour.

### Confirm the Deployment is running and uses the ConfigMap/Secret
```bash
kubectl get deployment todoapp -n todoapp
kubectl describe deployment todoapp -n todoapp
```

Look for the `Environment` section in the output — it should show:
- `PYTHONUNBUFFERED` sourced from `configmap todoapp-config`
- `SECRET_KEY` sourced from `secret todoapp-secret`

### Check that Pods are healthy
```bash
kubectl get pods -n todoapp
```

All pods should show `Running` status with `READY` containers (e.g. `1/1`).

### Validate environment variables inside a running Pod
```bash
# Replace <pod-name> with an actual pod name from the previous command
kubectl exec -it <pod-name> -n todoapp -- env | grep PYTHONUNBUFFERED
kubectl exec -it <pod-name> -n todoapp -- env | grep SECRET_KEY
```

Both variables should be printed with their expected values.

### Check that the app responds correctly
```bash
# Via NodePort (port 30007 as defined in nodeport.yml)
curl http://<NODE_IP>:30007/api/ready
curl http://<NODE_IP>:30007/api/health

# Or using the busybox pod inside the cluster
kubectl exec -it busybox -n todoapp -- curl todoapp-service.todoapp.svc.cluster.local/api/ready
```

---

## 4. Update settings.py (application change)

In the Django application's `settings.py`, replace the hardcoded `SECRET_KEY` with:

```python
import os
SECRET_KEY = os.environ.get('SECRET_KEY')
```

This ensures the application reads the secret from the environment variable injected by Kubernetes rather than using a hardcoded value.