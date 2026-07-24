# Instructions

## Applying the changes

Apply the manifests in this order — ConfigMap and Secret must exist before the Deployment, otherwise the pod will fail with `CreateContainerConfigError`:

```bash
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
```

## Validating the changes

1. Check that the pod is running:

```bash
kubectl get pods -n todoapp
```

Expected: the pod is in `Running` status with `1/1` ready.

2. Copy the pod name from the output above and check that the environment variables were injected correctly:

```bash
kubectl exec -it <pod-name> -n todoapp -- printenv | grep -E "SECRET_KEY|PYTHONUNBUFFERED"
```

Expected output:
PYTHONUNBUFFERED=1
SECRET_KEY=<value from the Secret>

This confirms that `PYTHONUNBUFFERED` is taken from the ConfigMap and `SECRET_KEY` from the Secret, and that the application no longer relies on the value hardcoded in `settings.py`.