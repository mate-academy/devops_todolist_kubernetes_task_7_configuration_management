# Instructions on how to apply configMap.yml and secret.yml to the cluster.

# How to deploy the configMap to your cluster:
kubectl apply -f configMap.yml -n todoapp

# How to deploy the secret to your cluster:
kubectl apply -f secret.yml -n todoapp

# How to verify the configMap and secret :
kubectl apply -f deployment.yml -n todoapp
kubectl get configmap -n todoapp
kubectl get secret -n todoapp

# How to validate the changes:
kubectl get pods -n todoapp
kubectl exec -it <pod-name> -n todoapp -- env | grep PYTHONUNBUFFERED
kubectl exec -it <pod-name> -n todoapp -- env | grep SECRET_KEY
