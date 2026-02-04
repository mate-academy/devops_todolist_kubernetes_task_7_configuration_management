# Deployment Instructions: TodoApp

This document provides steps to deploy the TodoApp and validate that the configurations (ConfigMaps and Secrets) are correctly applied.

## 1. Preparation
Navigate to the directory containing your Kubernetes manifests:

`cd .infrastructure/`

## 2. Apply Configurations
First, apply the environment settings and secrets so they are available for the deployment:

`kubectl apply -f configMap.yml -n todoapp`

`kubectl apply -f secret.yml -n todoapp`

## 3. Deploy the Application
Deploy the application:

`kubectl apply -f deployment.yml -n todoapp`

## 4. Validation (Verification of Changes)
A. Check Resource Status
Verify that the deployment is successful and all pods are running:

`kubectl get deployment todoapp -n todoapp`

`kubectl get pods -n todoapp`

B. Verify Environment Variables (ConfigMap & Secret Check)
To confirm that the environment variables from the ConfigMap and Secret have been correctly injected into the container, run the following command:

`kubectl exec -it deployment/todoapp -n todoapp -- printenv`

C. Verify Service Connectivity
Check if the service is reachable by forwarding the port to your local machine:
`kubectl port-forward deployment/todoapp -n todoapp 8081:8080`
- `Access the app at: http://localhost:8081`

