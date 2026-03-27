# CONFIGURATION INSTRUCTION 

## Apply manifests

```bash
kubectl apply -f .infrastructure/  
```

## Verify resources

```bash
kubectl -n todoapp get configmap todoapp-config -o yaml  
kubectl -n todoapp get secret todoapp-secret -o yaml  
kubectl -n todoapp get deploy,pods,svc  
```
## Validate env variables inside the pod

Get pod name:
```bash
kubectl -n todoapp get pods  
```

Check env variables:
```bash
kubectl -n todoapp exec -it pod/<POD_NAME> -- printenv | grep -E "PYTHONUNBUFFERED|SECRET_KEY"  
```

Expected:

- PYTHONUNBUFFERED=1 (from ConfigMap)  
- SECRET_KEY (from Secret)  

## Validate application endpoints
```bash
kubectl -n todoapp port-forward svc/todoapp-service 8080:80  
```

In another terminal:
```bash
curl -i http://localhost:8080/api/ready  
curl -i http://localhost:8080/api/health
```  