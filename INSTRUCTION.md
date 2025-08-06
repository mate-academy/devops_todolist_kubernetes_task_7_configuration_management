# ✅ How to Apply and Validate Kubernetes Configuration

## Step 1 — Apply ConfigMap
```bash
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml

kubectl exec -it <POD_NAME> -- printenv | grep PYTHONUNBUFFERED
kubectl exec -it <POD_NAME> -- printenv | grep SECRET_KEY

kubectl get pods
kubectl logs <POD_NAME>
