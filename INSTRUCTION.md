# Apply ConfigMap and Secret

Apply ConfigMap:

```bash id="v8k1re"
kubectl apply -f .infrastructure/configMap.yml
```

Apply Secret:

```bash id="y4p6cw"
kubectl apply -f .infrastructure/secret.yml
```

Restart deployment:

```bash id="m9x2jb"
kubectl rollout restart deployment todoapp -n mateapp
```

Check resources:

```bash id="c5q7td"
kubectl get configmap -n mateapp
kubectl get secret -n mateapp
```

---

# Validate ConfigMap

Check ConfigMap values:

```bash id="k2f8eu"
kubectl describe configmap todoapp-config -n mateapp
```

Check environment variables inside pod:

```bash id="x7r3qa"
kubectl exec -it <pod-name> -n mateapp -- env
```

Expected:

```text id="b4m6yn"
PYTHONUNBUFFERED=1
```

---

# Validate Secret

Check Secret:

```bash id="u1n9hv"
kubectl describe secret todoapp-secret -n mateapp
```

Check SECRET_KEY inside pod:

```bash id="j8p5lx"
kubectl exec -it <pod-name> -n mateapp -- env | grep SECRET_KEY
```

Expected:

```text id="f6s2ka"
SECRET_KEY=my-super-secret-key
```

---

# Validate Application

Check pods:

```bash id="r3m7uw"
kubectl get pods -n mateapp
```

Check application:

```bash id="e5k4zb"
kubectl port-forward svc/todoapp-clusterip 8080:80 -n mateapp
```

Open:

```text id="w7u9nj"
http://localhost:8080/api/health
http://localhost:8080/api/ready
```
