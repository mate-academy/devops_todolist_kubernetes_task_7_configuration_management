# Kubernetes Configuration Management Instructions

## Applying the Changes

1. Apply the ConfigMap:
```bash
kubectl apply -f .infrastructure/configmap.yml
```

2. Apply the Secret:
```bash
kubectl apply -f .infrastructure/secret.yml
```

3. Apply the updated Deployment:
```bash
kubectl apply -f .infrastructure/deployment.yml
```

## Validating the Changes

1. Verify the ConfigMap was created:
```bash
kubectl get configmap todoapp-config -n todoapp
```

2. Verify the Secret was created:
```bash
kubectl get secret todoapp-secret -n todoapp
```

3. Check if the pods are running with the new configuration:
```bash
kubectl get pods -n todoapp
```

4. Verify the environment variables in a running pod:
```bash
kubectl get pods -n todoapp

kubectl exec -n todoapp POD_NAME -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```

5. Check the pod logs to verify the application is using the new configuration:
```bash
kubectl logs -n todoapp POD_NAME
```