# TodoApp Configuration Management Instructions

## 1. Deploy to cluster

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/confgiMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
```

Verify resources are created:

```bash
kubectl get configmap -n todoapp
kubectl get secret -n todoapp
kubectl get deployment -n todoapp
kubectl get pods -n todoapp
```

---

## 2. Validate ConfigMap

Check that ConfigMap exists and contains the expected key:

```bash
kubectl describe configmap todoapp-config -n todoapp
```

You should see `PYTHONUNBUFFERED: 1` in the `Data` section.

---

## 3. Validate Secret

Check that Secret exists:

```bash
kubectl describe secret my-secret -n todoapp
```

You should see `SECRET_KEY` listed under `Data` (value is hidden for security).

---

## 4. Validate environment variables in the pod

Get a running pod name:

```bash
kubectl get pods -n todoapp
```

Exec into the pod and check that env vars are injected:

```bash
kubectl exec -it <pod-name> -n todoapp -- env | grep -E "SECRET_KEY|PYTHONUNBUFFERED"
```

You should see both variables printed with their correct values.
