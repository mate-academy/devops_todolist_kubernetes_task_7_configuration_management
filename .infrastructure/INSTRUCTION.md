# INSTRUCTION: Deploying TodoApp with ConfigMaps and Secrets

## 1. Prerequisites
Go to the `.infrastructure` directory
## 2. Applying Manifests
Apply the resources in the following order to ensure dependencies (ConfigMap and Secret) exist before the Deployment starts:
1. **Create the Namespace:**
```
kubectl apply -f namespace.yml
```
2. **Create Configuration Resources:**
```
kubectl apply -f confgiMap.yml
```

```
kubectl apply -f secret.yml
```
3. **Deploy the Application:**
```
kubectl apply -f deployment.yml
```
4. **Expose the Application:**
```
kubectl apply -f nodeport.yml
```
## 3. Validation Steps

### Check Resource Status
Verify that all resources are created and the Pod is running:
```
kubectl get all -n todoapp
```
### Verify Environment Variables Injection

To confirm that the values from the ConfigMap and Secret are correctly mapped to the container's environment, execute the following command:
```
kubectl exec -it $(kubectl get pod -l app=todoapp -n todoapp -o jsonpath='{.items[0].metadata.name}') -n todoapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```
**Expected Result:**
- `PYTHONUNBUFFERED=1` (sourced from `configmap.yml`)
- `SECRET_KEY=[your_decoded_key]` (sourced from `secret.yml`, automatically decoded from base64)
### Verify Application Logic
To ensure the Python application is actually using the `SECRET_KEY` from the environment instead of the hardcoded fallback, run:
```
kubectl exec -it $(kubectl get pod -l app=todoapp -n todoapp -o jsonpath='{.items[0].metadata.name}') -n todoapp -- python3 -c "import os; print(f'Active Secret Key: {os.environ.get(\"SECRET_KEY\")}')"
```
