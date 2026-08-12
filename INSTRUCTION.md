1. Apply the changes

Make sure you are inside the cloned repository folder.

# Apply ConfigMap
kubectl apply -f confgiMap.yml

# Apply Secret
kubectl apply -f secret.yml

# Apply Deployment
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
kubectl describe deployment todoapp


In the Environment section of the container, you should see:

PYTHONUNBUFFERED from ConfigMap

SECRET_KEY from Secret

Check running Pod
kubectl get pods
kubectl logs <todoapp-pod-name>


Logs should show the application is running correctly without errors.

3. Clean up (optional)

If you want to remove the resources:

kubectl delete -f confgiMap.yml
kubectl delete -f secret.yml
kubectl delete -f deployment.yml
