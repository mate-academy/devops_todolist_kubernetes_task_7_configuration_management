kubectl apply -f configMap.yml
kubectl get configmap -n todoapp


kubectl apply -f secret.yml
kubectl get secret -n todoapp
kubectl apply -f deployment.yml
kubectl get deployment -n todoapp
kubectl exec -it <pod name> -n todoapp -- sh
  printenv