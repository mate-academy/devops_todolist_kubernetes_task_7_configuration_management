To apply ConfigMap and Secret
```bash
cd .infrastructure
kubectl apply -f namespace.yml
kubectl apply -f configMap.yml -n todoapp
kubectl apply -f secret.yml -n todoapp
kubectl apply -f deployment.yml -n todoapp
```

To test that env variables are set properly
1. Get any pod of todo app
```bash
kubectl get pods
```
2. Go to shell
```bash
kubectl exec -it <pod_id> -- sh
```
3. Call printenv inside shell
