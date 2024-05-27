To apply the changes, use the following commands:

Create the ConfigMap:

sh
Копіювати код
kubectl apply -f configMap.yml
Create the Secret:

sh
Копіювати код
kubectl apply -f secret.yml
Deploy the updated application:

sh
Копіювати код
kubectl apply -f deployment.yml
Validating the Changes
To validate the changes, follow these steps:

Ensure the ConfigMap and Secret have been created:

sh
Копіювати код
kubectl get configmaps -n todoapp
kubectl get secrets -n todoapp
Check the environment variables in the deployed pod:

sh
Копіювати код
kubectl exec -it <pod-name> -n todoapp -- printenv | grep PYTHONUNBUFFERED
kubectl exec -it <pod-name> -n todoapp -- printenv | grep SECRET_KEY
Verify the application is running correctly:

Access the application via the service URL or NodePort.
Check the logs of the pod to ensure there are no errors related to missing environment variables:
sh
Копіювати код
kubectl logs <pod-name> -n todoapp
Confirm the liveness and readiness probes are functioning as expected:

sh
Копіювати код
kubectl describe pod <pod-name> -n todoapp
Testing the Application
To test the ToDo application, you can use the following commands:

Use port-forward to access the application locally:

sh
Копіювати код
kubectl port-forward svc/todoapp-service -n todoapp 8080:80
Access the application by opening a browser and navigating to http://localhost:8080.

Use a busybox container to test the ClusterIP service DNS:

sh
Копіювати код
kubectl run -i --tty busybox --image=busybox --restart=Never -- sh
nslookup todoapp-service.todoapp.svc.cluster.local
Check the application logs for further debugging:

sh
Копіювати код
kubectl logs -l app=todolist -n todoapp