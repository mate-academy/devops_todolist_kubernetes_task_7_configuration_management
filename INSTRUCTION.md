# ToDo App Kubernetes Deployment Instructions

## 1. Apply the changes
To deploy the application and its configurations to the Kubernetes cluster, execute the following commands in your terminal:

kubectl create namespace todoapp --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml

## 2. Validate the changes
To ensure that all resources have been successfully created and the application is running, run the following validation commands:

kubectl get configmap app-config -n todoapp
kubectl get secret app-secret -n todoapp
kubectl get pods -n todoapp