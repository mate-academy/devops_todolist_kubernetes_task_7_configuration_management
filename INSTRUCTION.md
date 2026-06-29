# Instruction

## Apply Changes

### 1. Apply ConfigMap
```bash
kubectl apply -f configMap.yml
```

### 2. Apply Secret
```bash
kubectl apply -f secret.yml
```

### 3. Apply Deployment
```bash
kubectl apply -f .infrastructure/deployment.yml
```

## Validate Changes

### Check ConfigMap
```bash
kubectl get configmap todoapp-config
kubectl describe configmap todoapp-config
```

### Check Secret
```bash
kubectl get secret todoapp-secret
kubectl describe secret todoapp-secret
```

### Check Pod environment variables
```bash
# Get pod name
kubectl get pods

# Check env variables inside the pod
kubectl exec -it <pod-name> -- env | grep PYTHONUNBUFFERED
kubectl exec -it <pod-name> -- env | grep SECRET_KEY
```

### Check Pod status
```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```