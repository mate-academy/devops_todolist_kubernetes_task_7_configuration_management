# Instructions

## Apply

Apply the configuration objects:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

Or apply all at once:

```bash
kubectl apply -f .infrastructure/
```

## Validate

### 1. Check ConfigMap and Secret exist

```bash
kubectl -n todoapp get configmap todoapp-config -o yaml
kubectl -n todoapp get secret todoapp-secret -o yaml
```

### 2. Verify env vars are injected

```bash
kubectl -n todoapp exec deploy/todoapp -- sh -c 'env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"'
```

Expected output:
- `PYTHONUNBUFFERED=1`
- `SECRET_KEY=<base64 value>`

### 3. Verify the app uses the secret (not hardcoded)

```bash
kubectl -n todoapp exec deploy/todoapp -- sh -c 'cd /app && python -c "import os; from todolist import settings; assert settings.SECRET_KEY == os.environ[\"SECRET_KEY\"]; print(\"secret key verified\")"'
```

### 4. Check app logs for startup

```bash
kubectl -n todoapp logs deploy/todoapp
```

### 5. Verify no Django SECRET_KEY warning

```bash
kubectl -n todoapp logs deploy/todoapp | grep -i "secret"
```

No "SECRET_KEY" warnings should appear.
