Instructions to Deploy and Validate ConfigMap and Secret in todoapp
Apply Resources
Apply ConfigMap
kubectl apply -f configMap.yml
Apply Secret
kubectl apply -f secret.yml
Apply Deployment
kubectl apply -f deployment.yml
Validate Resources
1. Check if ConfigMap and Secret were created:
kubectl get configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp
2. Check the pod:
kubectl get pods -n todoapp
3. Inspect environment variables in the running pod:
kubectl exec -it <pod-name> -n todoapp -- env | grep PYTHONUNBUFFERED
kubectl exec -it <pod-name> -n todoapp -- env | grep SECRET_KEY
4. Confirm the app is running and responding:
kubectl port-forward svc/todoapp 8080:8080 -n todoapp
curl http://localhost:8080/api/ready
curl http://localhost:8080/api/health