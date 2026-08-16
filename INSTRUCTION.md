# Kubernetes ToDo App Instructions

## 1. Create the namespace
```bash
kubectl apply -f .infrastructure/namespace.yml
```

Verify that the namespace was created:
```bash
kubectl get namespace todoapp
```

## 2. Apply the ConfigMap
```bash
kubectl apply -f .infrastructure/configMap.yml
```

Verify the ConfigMap:
```bash
kubectl get configmap todoapp -n todoapp
kubectl describe configmap todoapp -n todoapp
```

## 3. Apply the Secret
```bash
kubectl apply -f .infrastructure/secret.yml
```

Verify the Secret:
```bash
kubectl get secret todoapp -n todoapp
kubectl describe secret todoapp -n todoapp
```

## 4. Apply the Deployment
```bash
kubectl apply -f .infrastructure/deployment.yml
```

Verify the Deployment:
```bash
kubectl get deployment todoapp -n todoapp
kubectl describe deployment todoapp -n todoapp
```

## 5. Validate the Pods

Check the Pods:
```bash
kubectl get pods -n todoapp
```

The Pod should have the `Running` status.

For more detailed information about a Pod:
```bash
kubectl describe pod -n todoapp
```

Check the application logs:
```bash
kubectl logs -n todoapp -l app=todolist
```

## 6. Validate environment variables

The Deployment loads `PYTHONUNBUFFERED` from the ConfigMap and `SECRET_KEY` from the Secret.

Get the name of the running Pod:
```bash
kubectl get pods -n todoapp
```

## 7. Validate the Service

```bash
kubectl apply -f .infrastructure/clusterIP.yml
```

Check the Service:
```bash
kubectl get svc -n todoapp
```

Describe the Service:
```bash
kubectl describe svc todoapp-service -n todoapp
```

## 8. Test the application endpoint

Forward the Service port to the local machine:

```bash
kubectl port-forward svc/todoapp-service 8080:80 -n todoapp
```

Then test the application:
```bash
curl http://localhost:8080
```

## 9. Validate all resources

```bash
kubectl get all -n todoapp
```
