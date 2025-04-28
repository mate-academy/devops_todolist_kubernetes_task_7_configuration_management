# Instructions with commands to apply all the changes

### Attached files
 
- confgiMap.yml
- secret.yml
- INSTRUCTION.md

### Apply ConfigMap

```bash
kubectl apply -f confgiMap.yml
```

### Apply Secret

```bash
kubectl apply -f secret.yml
```

### Apply Deployment
```bash
kubectl apply -f deployment.yml
```

## Check if ConfigMap and Secret were created:

```bash
kubectl get configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp
```

### Check the pod:

```bash
kubectl get pods -n todoapp
```

### Environment variables in the running pod:

```bash
kubectl exec -it <pod-name> -n todoapp -- env | grep PYTHONUNBUFFERED
kubectl exec -it <pod-name> -n todoapp -- env | grep SECRET_KEY
