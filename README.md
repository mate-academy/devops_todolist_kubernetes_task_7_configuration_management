## Apply all manifests

```
kubectl apply -f .infrastructure/
```

## How to validate the changes

```
kubectl -n todoapp get deployments
```

```
kubectl -n todoapp get pods
```

```
kubectl -n todoapp logs <pod_name>
```
