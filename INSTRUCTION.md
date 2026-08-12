### 1. Apply the Manifests

```bash
# Create the namespace first
kubectl apply -f .infrastructure/namespace.yml

# Create the ConfigMap and Secret
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml

# Deploy the application and its service
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/nodeport.yml
```

### 2. Validate the Resources

Check the ConfigMap
```bash
kubectl get configmap mateapp-configmap -n todoapp -o yaml
```

Check the Secret (the value is base64 encoded)
```bash
kubectl get secret mateapp-secret -n todoapp -o yaml
```

Decode the secret to verify its actual value
```bash
kubectl get secret mateapp-secret -n todoapp -o jsonpath='{.data.SECRET_KEY}' | base64 --decode
```
