Clone repository
git clone repository
go to folder repository

## Deploy secret and config
create namespace
kubectl apply -f namespace.yml
check namespaces
kubectl get ns
create secret
kubectl apply -f secret.yml
create config 
kubectl apply -f configMap.yml

## DEPLOY
create deployment
kubectl apply -f deployment.yml
check replica
kubectl get pods -n mateapp -o wide

## DEPLOY FOR CHECKING
create busybox
kubectl apply -f busybox.yml
create service clusterip
kubectl apply -f clusterIp.yml
create node port
kubectl apply -f nodeport.yml

## TESTING
check key
kubectl get secret app-secrets -o jsonpath=’{.data.*}’
check env
kubectl exec -it <name pod> -- sh  
printenv
