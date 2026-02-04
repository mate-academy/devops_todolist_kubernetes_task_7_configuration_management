### Instruction: TodoApp Deployment with Configs and Secrets

##### Open your terminal and navigate to the directory containing your Kubernetes manifests:

`cd .infrastructure/`

##### 1. Apply Configurations:
Apply ConfigMap:
`kubectl apply -f configMap.yml -n todoapp`

Apply Secret:
`kubectl apply -f secret.yml -n todoapp`

##### 2. Deploy the Application
`kubectl apply -f deployment.yml -n todoapp`
