# ToDo app: ConfigMap and Secret

- `.infrastructure/configMap.yml`: ConfigMap `todoapp-config` with `PYTHONUNBUFFERED=1`.
- `.infrastructure/secret.yml`: Secret `todoapp-secret` with the Django `SECRET_KEY` (base64-encoded in `data`).
- `.infrastructure/deployment.yml`: the container gets `PYTHONUNBUFFERED` from the ConfigMap (`configMapKeyRef`) and `SECRET_KEY` from the Secret (`secretKeyRef`).
- `src/todolist/settings.py`: the hardcoded key is removed, the app reads it from the environment: `SECRET_KEY = os.environ.get("SECRET_KEY")`.

## 1. Build and push the image

The code in `settings.py` changed, so the app needs a new image:

```bash
docker build -t kkurama/todoapp:4.0.0 src
docker push kkurama/todoapp:4.0.0
```

## 2. Apply all the changes

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
```

The ConfigMap and the Secret must be applied before the Deployment: a pod that references a missing ConfigMap or Secret stays in `CreateContainerConfigError`.

Wait until the pods are ready (~30 seconds after start):

```bash
kubectl rollout status deployment/todoapp -n todoapp
kubectl get pods -n todoapp
```

## 3. Validate the changes

### ConfigMap and Secret exist

```bash
kubectl get configmap todoapp-config -n todoapp -o yaml
kubectl get secret todoapp-secret -n todoapp
kubectl describe secret todoapp-secret -n todoapp
```

`describe` shows `SECRET_KEY: 50 bytes` without revealing the value. To decode it:

```bash
kubectl get secret todoapp-secret -n todoapp -o jsonpath='{.data.SECRET_KEY}' | base64 -d; echo
```

### Environment variables inside the container

```bash
kubectl exec -n todoapp deploy/todoapp -- printenv PYTHONUNBUFFERED SECRET_KEY
```

Expected: `1` and the same key as decoded from the Secret.

### Django uses the key from the Secret

```bash
kubectl exec -n todoapp deploy/todoapp -- python manage.py shell -c "from django.conf import settings; print(settings.SECRET_KEY)"
```

The output must match the value from the Secret, not the old hardcoded key from `settings.py`.

### The app works

```bash
kubectl port-forward service/todoapp-service 8080:80 -n todoapp
```

In another terminal:

```bash
curl http://localhost:8080/api/health
```

Expected: `Health OK`. The landing page is available at http://localhost:8080.
