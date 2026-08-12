Apply ConfigMap and Secrets requirements:
```bash
- kubectl apply -f configMap.yml
- kubectl apply -f secret.yml
```

```bash
- kubectl get configmap -n todoapp
- kubectl get secret -n todoapp
```

```bash
kubectl exec -it <pod_name> -n todoapp -- printenv | grep PYTHONUNBUFFERED
```