## 5. ConfigMaps and Secrets configuration

### Apply Configuration
To apply the ConfigMap and Secret, and update the Deployment to use them:

```
# 1. Apply ConfigMap
kubectl apply -f .infrastructure/configiMap.yml

# 2. Apply Secret
kubectl apply -f .infrastructure/secret.yml

# 3. Update Deployment (to inject new env vars)
kubectl apply -f .infrastructure/deployment.yml
```
### Validation
1. Validate Environment Variables inside the Pod Check if the variables are correctly injected into the container.

```
# Get one of the pod names
kubectl get pods -n mateapp

# Execute 'env' command inside the pod
kubectl exec -it <pod-name> -n mateapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```
Expected output:
```
Plaintext
PYTHONUNBUFFERED=1
SECRET_KEY=super-secret-key-123