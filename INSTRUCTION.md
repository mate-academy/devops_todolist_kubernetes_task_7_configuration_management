# Instructions for Applying and Validating Changes

## Apply Changes

1. Apply the configuration using kubectl:
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f configmap.yaml
```

## Validate Changes

1. Check if the deployment was successful:
```bash
kubectl get deployments
kubectl get pods
```

2. Verify the ConfigMap:
```bash
kubectl get configmaps
kubectl describe configmap todolist-config
```

3. Check the service:
```bash
kubectl get services
```

4. Validate the application:
```bash
# Get the service URL
kubectl get service todolist-service

# Access the application using the external IP/port
curl http://<EXTERNAL-IP>:<PORT>
```

5. Check pod logs for any errors:
```bash
kubectl logs -l app=todolist
```