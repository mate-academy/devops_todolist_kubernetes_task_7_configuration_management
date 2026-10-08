# Instructions for Deploying and Testing ConfigMap and Secret

## 1. How to Apply Manifests
```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

## 2. How to Validate the Changes
```bash
# Verify resources exist
kubectl get configmap todoapp-config -n mateapp
kubectl get secret todoapp-secret -n mateapp

# Check environment variables inside one of the deployment pods
kubectl exec -it deployment/todoapp-deployment -n mateapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```
