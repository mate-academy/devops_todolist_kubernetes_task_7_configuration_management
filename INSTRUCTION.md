```
kubectl apply -f .infrastructure/configMap.yml -n todoapp
```

```
kubectl apply -f .infrastructure/secret.yml -n todoapp
```

```
kubectl apply -f .infrastructure/deployment.yml -n todoapp
```

get name of just created pod

```
kubectl get pods -n todoapp
```

connect to one of the created pods with command

```
kubectl exec -it -n todoapp <POD_NAME> -- sh
```

after connecting to the pod run command to check the list of env vars

```
printenv
```
