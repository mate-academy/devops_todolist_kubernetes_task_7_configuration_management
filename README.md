# Apply changes

kubectl apply -f configmap.yml

kubectl apply -f secret.yml

kubectl apply -f deployment.yml 

# Validate changes

kubectl get deployments -n todoapp

kubectl get pods -n todoapp

kubectl logs <pod_name> -n todoapp
