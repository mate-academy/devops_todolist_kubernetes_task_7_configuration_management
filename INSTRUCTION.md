# Instruction

## Build and push the image

```bash
cd src
docker build -t drdr92/todoapp:4.0.0 .
docker push drdr92/todoapp:4.0.0
cd ..
```

## Apply the manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

## Validate the changes

Check that the pod is running:

```bash
kubectl get pods -n todoapp
```

Check that the ConfigMap and the Secret exist:

```bash
kubectl get configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp
```

Check that the environment variables are set inside the container:

```bash
kubectl exec -n todoapp deploy/todoapp -- printenv PYTHONUNBUFFERED SECRET_KEY
```

Expected output:

```
1
SUPERMEGA_SECRET_KEY
```