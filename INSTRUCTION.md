# INSTRUCTION

## 1. Deploy ConfigMap and Secret

Create the namespace:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Apply ConfigMap and Secret:

```bash
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
```

## 2. Deploy the application

Apply the Deployment:

```bash
kubectl apply -f .infrastructure/deployment.yml
```

Check the resources:

```bash
kubectl get configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp
kubectl get deployment todoapp -n todoapp
kubectl get pods -n todoapp
```

## 3. Validate ConfigMap and Secret

Verify that the Deployment references ConfigMap and Secret:

```bash
kubectl describe deployment todoapp -n todoapp
```

Check that the environment variables are available inside the application container:

```bash
kubectl exec -n todoapp deployment/todoapp -- sh -c 'test "$PYTHONUNBUFFERED" = "1" && echo "ConfigMap OK"'
```

Verify that `SECRET_KEY` is set without displaying its value:

```bash
kubectl exec -n todoapp deployment/todoapp -- sh -c 'test -n "$SECRET_KEY" && echo "Secret OK"'
```

Expected results:

```text
ConfigMap OK
Secret OK
```

## 4. Validate Django configuration

The application reads its `SECRET_KEY` from the environment variable using:

```python
SECRET_KEY = os.environ.get("SECRET_KEY")
```

The value is provided by Kubernetes Secret rather than hardcoded in `settings.py`.

**Note:** The container image must be rebuilt and redeployed after modifying `settings.py`, otherwise the running application may still contain the previous code.
