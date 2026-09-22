# ConfigMap and Secret Deployment

## Apply resources

Create ConfigMap:

```bash
kubectl apply -f configMap.yml
```

Create Secret:

```bash
kubectl apply -f secret.yml
```

Apply Deployment:

```bash
kubectl apply -f deployment.yml
```

## Validation

Verify ConfigMap:

```bash
kubectl get configmap -n mateapp
kubectl describe configmap todoapp-config -n mateapp
```

Verify Secret:

```bash
kubectl get secret -n mateapp
kubectl describe secret todoapp-secret -n mateapp
```

Verify environment variables inside a pod:

```bash
kubectl exec -it <pod-name> -n mateapp -- env
```

The output should contain:

```text
PYTHONUNBUFFERED=1
SECRET_KEY=<value>
```

Verify Deployment:

```bash
kubectl get deployments -n mateapp
kubectl get pods -n mateapp
```
