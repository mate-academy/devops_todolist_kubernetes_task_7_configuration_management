# 1. Make sure that `namespace` exists:
```bash
kubectl apply -f .infrastructure/namespace.yml
```

# 2. Apply secrets:
```bash
kubectl apply -f .infrastructure/secret.yaml
```

# 3. Apply config:
```bash
kubectl apply -f .infrastructure/configMap.yaml
```

# 4. Apply deployment:
```bash
kubectl apply -f deployment.yml
```

# 5. Validation:
- Validate env app via connect to any pod:
```bash
kubectl exec -it POD_NAME --sh
```

- Execute shell command:
```bash 
printenv
```
- Validate secret by command:
```bash
kubectl get secret app-secrets -o jsonpath='{.data.*}'
```