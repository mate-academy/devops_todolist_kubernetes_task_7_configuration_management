# 🐳 Build and Deploy Updated Docker Image for Django ToDo App

## 1. Prerequisites

- Docker is installed and running
- You are authenticated in Docker Hub:
  ```bash
  docker login
  ```
- You are in the `src/` directory with the following structure:
  - `Dockerfile`
  - `manage.py`
  - `requirements.txt`
  - Django apps (`todolist/`, `lists/`, etc.)
- You have Kubernetes configured and a cluster running

---

## 2. Build the Image

Build a new image with tag `3.0.1`:

```bash
docker build -t olhadevandops/todoapp:3.0.1 .
```

---

## 3. Push the Image to Docker Hub

```bash
docker push olhadevandops/todoapp:3.0.1
```

You should see output like:

```
The push refers to repository [docker.io/olhadevandops/todoapp]
...
3.0.1: digest: sha256:...
```

---

## 4. Prepare Kubernetes Resources

### Create Namespace

```bash
kubectl create namespace todoapp --dry-run=client -o yaml | kubectl apply -f -
```

### Apply ConfigMap

```bash
kubectl apply -f .infrastructure/configMap.yml
```

### Apply Secret

```bash
kubectl apply -f .infrastructure/secret.yml
```

---

## 5. Deploy the Updated App

Update the container image in `.infrastructure/deployment.yml`:

```yaml
containers:
  - name: todoapp
    image: olhadevandops/todoapp:3.0.1
```

Apply the updated deployment:

```bash
kubectl apply -f .infrastructure/deployment.yml
```

Or perform a rollout restart:

```bash
kubectl rollout restart deployment todoapp -n todoapp
```

---

## 6. Verify Deployment

### Check pod status:

```shell
kubectl get pods -n todoapp
```

Result:
```shell
NAME                       READY   STATUS    RESTARTS   AGE
todoapp-5f569f9bbf-zzbwf   1/1     Running   0          37s
```

### View logs:

```shell
kubectl logs -n todoapp todoapp-5f569f9bbf-zzbwf
```
Result:
```shell
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
May 27, 2025 - 19:15:17
Django version 4.1.10, using settings 'todolist.settings'
Starting development server at http://0.0.0.0:8080/
Quit the server with CONTROL-C.
Internal Server Error: /api/ready
[27/May/2025 19:15:19] "GET /api/ready HTTP/1.1" 500 17
Internal Server Error: /api/ready
[27/May/2025 19:15:24] "GET /api/ready HTTP/1.1" 500 17
Internal Server Error: /api/ready
[27/May/2025 19:15:29] "GET /api/ready HTTP/1.1" 500 17
Internal Server Error: /api/ready
[27/May/2025 19:15:34] "GET /api/ready HTTP/1.1" 500 17
Internal Server Error: /api/ready
[27/May/2025 19:15:39] "GET /api/ready HTTP/1.1" 500 17
Internal Server Error: /api/ready
[27/May/2025 19:15:44] "GET /api/ready HTTP/1.1" 500 17
[27/May/2025 19:15:49] "GET /api/ready HTTP/1.1" 200 12
[27/May/2025 19:15:54] "GET /api/ready HTTP/1.1" 200 12
[27/May/2025 19:15:59] "GET /api/ready HTTP/1.1" 200 12
[27/May/2025 19:16:04] "GET /api/ready HTTP/1.1" 200 12
[27/May/2025 19:16:09] "GET /api/ready HTTP/1.1" 200 12
```

### Verify env variable is set:

#### Check both environment variables from ConfigMap and Secret
```shell
kubectl exec -n todoapp -it todoapp-5f569f9bbf-zzbwf -- printenv | grep SECRET_KEY
kubectl exec -n todoapp -it todoapp-5f569f9bbf-zzbwf -- printenv | grep PYTHONUNBUFFERED
```
Result:
```shell
SECRET_KEY=eW91ci12ZXJ5LXNlY3JldC1rZXk=
PYTHONUNBUFFERED=1
```
---

## 🧹 Cleanup Instructions

### ❌ Delete all resources in `todoapp` namespace:

```bash
kubectl delete all --all -n todoapp
```

This will remove all: Pods, Deployments, Services, HPAs, etc.

### ❌ Delete the namespace itself:

```bash
kubectl delete namespace todoapp
```

If the namespace remains stuck in `Terminating`, run:

```bash
kubectl get namespace todoapp -o json \
| jq 'del(.spec.finalizers)' \
| kubectl replace --raw "/api/v1/namespaces/todoapp/finalize" -f -
```

### ✅ Confirm everything is deleted:

```bash
kubectl get all -A | grep todoapp
```

---
