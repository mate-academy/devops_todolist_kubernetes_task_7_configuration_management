1. Apply the changes

First, make sure you are inside the cloned repository folder.

# Apply ConfigMap
kubectl apply -f configMap.yml

# Apply Secret
kubectl apply -f todo-secret.yml

# Apply Deployment (the file that defines the ToDo app pod/container)
kubectl apply -f deployment.yml

2. Validate the changes
Check that ConfigMap is created
kubectl get configmap
kubectl describe configmap todo-config


You should see the key:

PYTHONUNBUFFERED=1

Check that Secret is created
kubectl get secret
kubectl describe secret todo-secret


Note: the value is stored in base64. To decode:

kubectl get secret todo-secret -o jsonpath='{.data.SECRET_KEY}' | base64 --decode

Check that the Deployment is using ConfigMap and Secret
kubectl describe deployment todo-deployment


In the Environment section of the container, you should see:

PYTHONUNBUFFERED from ConfigMap

SECRET_KEY from Secret

Check running Pod
kubectl get pods
kubectl logs


Logs should show the application is running correctly without errors.

3. Clean up (optional)

If you want to remove the resources:

kubectl delete -f configMap.yml
kubectl delete -f secret.yml
kubectl delete -f deployment.yml