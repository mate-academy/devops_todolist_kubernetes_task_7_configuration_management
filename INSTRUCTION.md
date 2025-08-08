# How to apply ConfigMap and Secret resources

## Create resources

### 1. Create namespace (if doesn't exist)

```shell
kubectl apply -f .infrastructure/namespaces.yml
```

### 2. Create ConfigMap

```shell
kubectl apply -f .infrastructure/configMap.yml
```

### 3. Create Secret

```shell
kubectl apply -f .infrastructure/secret.yml
```

### 4. Apply Deployment

```shell
kubectl apply -f .infrastructure/deployment.yml
```

## Verify changes

### 1. Go inside one of the pods

```shell
kubectl get pods -n todoapp # Get name of one of the pods
```

```shell
kubectl exec -it <name_of_pod> -n todoapp -- sh 
```

### 2. Find `SECRET_KEY` and `PYTHONUNBUFFERED` environment variables at output

```shell
printenv
```
