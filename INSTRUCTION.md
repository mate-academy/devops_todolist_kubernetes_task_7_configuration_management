### Apply all the changes

kubectl apply -f configMap.yml 
kubectl apply -f secret.yml
kubectl apply -f deployment.yml



### How to validate the changes

kubectl get secret
kubectl describe secret app-secret

kubectl rollout status deployment todoapp
kubectl exec -it <pod-name> -- /bin/sh
echo $SECRET_KEY
