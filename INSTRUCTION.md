# Kubernetes ToDo App Instructions

Create the `todoapp` namespace:
```bash
kubectl apply -f .infrastructure/namespace.yml
```

Apply the ConfigMap:
```bash
kubectl apply -f .infrastructure/configMap.yml
```

Apply the Secret:
```bash
kubectl apply -f .infrastructure/secret.yml
```

Apply the Deployment:
```bash
kubectl apply -f .infrastructure/deployment.yml
```

## Validate the resources
Check that the namespace exists:
```bash
kubectl get namespaces
```

Check the ConfigMap:
```bash
kubectl get configmap todoapp -n todoapp
```

Check the Secret:
```bash
kubectl get secret todoapp -n todoapp
```

Check the Deployment:
```bash
kubectl get deployment -n todoapp
```

Check the Pods:
```bash
kubectl get pods -n todoapp
```

## Validate environment variables

Get the name of the running Pod:
```bash
kubectl get pods -n todoapp
```

## Validate the application

Check that the Pod is running:
```bash
kubectl get pods -n todoapp
```
The Pod should have the `Running` status.
