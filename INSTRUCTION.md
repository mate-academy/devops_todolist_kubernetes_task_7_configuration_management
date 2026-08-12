# Django ToDo List
## Applying all changes
```bash
cd .infrastructure/
```
```bash
kubectl apply -f configMap.yml
```
```bash
kubectl apply -f secret.yml
```
```bash
kubectl apply -f deployment.yml
```
## Access the application
Run this commands
```bash
kubectl get configMap app-config -n todoapp
```
DATA column should be equal 1
Same for secrets
```bash
kubectl get secrets app-secret -n todoapp
```
Run this command to see your `SECRET_KEY`:
```bash
kubectl get secret app-secret -o jsonpath='{.data.*}'
```
### Looking for new enviromental variabls inside pod
```bash
kubectl get pods -n todoapp
```
Name of the container will look like todoapp-< random values >-< random values >
After run this command
```bash
kubectl exec -it todoapp-< random values >-< random values > -n todoapp -- sh 
```
Inside the container run this command to check enviromentable variables inside the container
```bash
printenv | grep PYTHONUNBUFFERED
```
```bash
printenv | grep SECRET_KEY
```