1. Recreate and check features from previous tasks

```
kubectl apply -f .infrastructure/namespace.yml
kubectl config set-context --current --namespace=mateapp
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl get svc -o wide
```

2. Create and check configMap setup

```
kubectl apply -f .infrastructure/configMap.yml
kubectl get configmap -o wide
```

3. Create and check secret setup

```
kubectl apply -f .infrastructure/secret.yml
kubectl get configmap -o wide
```

4. Create and check successfully created deployment

```
kubectl apply -f .infrastructure/deployment.yml
kubectl get deployments -o wide
kubectl get pods -o wide
```

5. Now you can enjoy fantastic todo application on http://localhost:30007/ or http://127.0.0.1:30007
