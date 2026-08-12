# Instructions to apply and validate Kubernetes ConfigMap, Secret, and deployment changes

## 1. Apply the ConfigMap
```
kubectl apply -f .infrastructure/configMap.yml
```

## 2. Apply the Secret
```
kubectl apply -f .infrastructure/secret.yml
```

## 3. Apply the Deployment
```
kubectl apply -f .infrastructure/deployment.yml
```

## 4. Validate the changes

### a. Get the pod name
```
kubectl get pods -n todoapp
```

### b. Enter the pod
```
kubectl exec -it <pod-name> -n todoapp -- /bin/sh
```

### c. Print environment variables
```
printenv | grep -E 'SECRET_KEY|PYTHONUNBUFFERED'
```

You should see the `SECRET_KEY` and `PYTHONUNBUFFERED` environment variables set as expected.
