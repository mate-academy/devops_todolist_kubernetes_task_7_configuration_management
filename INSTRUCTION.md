## Apply ConfigMap and Secret

```bash
cd .infrastructure
kubectl apply -f confgiMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
```

⚠️ All the files should be in the same directory, or specify the path to them

## How to know if the changes have been applied?

1. Verify, if ConfigMap was created:

```bash
kubectl get configmap
kubectl describe configmap app-config
```

2. Verify, if Secret was created (the same as ConfigMap):

```bash
kubectl get secret
kubectl describe secret app-secret
```

3. Verify, if pods took the environment variables:

```bash
kubectl get pods
kubectl exec -it <pod-name> -- /bin/sh
```

Check variables:

```bash
echo $PYTHONUNBUFFERED
echo $SECRET_KEY
```