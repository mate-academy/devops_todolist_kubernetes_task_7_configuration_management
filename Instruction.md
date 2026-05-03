# Deployment Instructions
## Apply Kubernetes resources
```bash
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
Validate changes
Check that ConfigMap was created:
kubectl get configmap -n mateapp
Check that Secret was created:
kubectl get secret -n mateapp
Check that Pod is running:
kubectl get pods -n mateapp
Check Pod logs:
kubectl logs <pod-name> -n mateapp
Check environment variables inside Pod:
kubectl exec <pod-name> -n mateapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"