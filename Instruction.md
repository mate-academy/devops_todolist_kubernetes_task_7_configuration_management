# Deployment Instructions
## Apply Kubernetes resources
```bash
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
Validate changes
Check that ConfigMap was created:
kubectl get configmap -n todoapp
Check that Secret was created:
kubectl get secret -n todoapp
Check that Pod is running:
kubectl get pods -n todoapp
Check Pod logs:
kubectl logs <pod-name> -n todoapp
Check environment variables inside Pod:
kubectl exec <pod-name> -n todoapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
kubectl get secret app-secrets -n todoapp
