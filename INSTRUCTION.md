```
kubectl apply -f .infrastructure/configMap.yml -n todoapp
```

```
kubectl apply -f .infrastructure/secret.yml -n todoapp
```

```
kubectl apply -f .infrastructure/deployment.yml -n todoapp
```

connect to one of the created pods with command

```
kubectl exec -it todoapp-6bc4b4fbcb-js74n -- sh -n todoapp
```

after connecting to the pod run command to check the list of env vars

```
printenv
```
