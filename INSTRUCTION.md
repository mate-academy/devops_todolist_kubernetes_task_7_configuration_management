kubectl apply -f confgiMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml

kubectl exec -it <todoapp-pod> -- sh  
printenv
