# Apply files
```
kubectl apply -f .infrastructure/confgiMap.yml
```

```
kubectl apply -f .infrastructure/secret.yml
```
```
kubectl apply -f .infrastructure/deployment.yml
```
## Check
```
kubectl get configmap
```

```
kubectl get secrets
```

```
kubectl get pod
```

Check env variables inside a pod:

```
kubectl exec -it todoapp-{random symbols} -- sh  
```

```
printenv
