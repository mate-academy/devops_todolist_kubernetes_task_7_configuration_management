# Deployment Instructions

## Prerequisites

- `kubectl` installed and configured
- Access to the target Kubernetes cluster
- Namespace `todoapp` already exists (or run the step below)

```bash
kubectl create namespace todoapp
```

---

## Apply All Resources

Apply the files in the following order:

```bash
# 1. Apply the ConfigMap
kubectl apply -f confgiMap.yml

# 2. Apply the Secret
kubectl apply -f secret.yml

# 3. Apply the Deployment
kubectl apply -f deployment.yml
```

---

## Validate the Changes

### 1. Verify the ConfigMap

```bash
kubectl get configmap todo-app-config -n todoapp
kubectl describe configmap todo-app-config -n todoapp
```

Expected: `PYTHONUNBUFFERED`, `ENVIRONMETN`, and `PORT` keys are present.

---

### 2. Verify the Secret

```bash
kubectl get secret todo-app-secret -n todoapp
kubectl describe secret todo-app-secret -n todoapp
```

Expected: `todo-app-secret` of type `Opaque` exists with a `SECRET_KEY` entry.

To decode and inspect the secret value:

```bash
kubectl get secret todo-app-secret -n todoapp \
  -o jsonpath="{.data.SECRET_KEY}" | base64 --decode
```

---

### 3. Verify the Deployment

```bash
kubectl get deployment todoapp -n todoapp
kubectl describe deployment todoapp -n todoapp
```

Expected: deployment is available and the environment variables reference the ConfigMap and Secret.

---

### 4. Verify Environment Variables Inside the Running Pod

```bash
# Get the pod name
kubectl get pods -n todoapp

# Exec into the pod and check env vars
kubectl exec -it <pod-name> -n todoapp  -- sh
printenv
```

Expected output:

```
PYTHONUNBUFFERED=1
SECRET_KEY=<decoded-secret-value>
```

---

### 5. Verify Pod Health

```bash
kubectl get pods -n todoapp
```

All pods should show `Running` status with `READY` state (e.g. `1/1`).

Check liveness and readiness probe results:

```bash
kubectl describe pod <pod-name> -n todoapp
```

Look for `Liveness` and `Readiness` sections — both should report successful HTTP GET responses on `/api/health` and `/api/ready`.
