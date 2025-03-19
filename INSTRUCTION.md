To apply the changes you need to rebuild the Docker container:
```
docker build -t ikulyk404/todoapp:3.0.0 src/
```
Apply the configMap and secret:
```
kubectl apply -f .\.infrastructure\confgiMap.yml
kubectl apply -f .\.infrastructure\secret.yml
start the application itself:
kubectl apply -f .\.infrastructure\deployment.yml
```

To check if the application works properly apply nodeport:
```
kubectl apply -f .infrastructure/nodeport.yml
```
Now you can connect using a browser to 
```
http://127.0.0.1:30007
```
