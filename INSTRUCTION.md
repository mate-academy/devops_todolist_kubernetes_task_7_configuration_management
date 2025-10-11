## How to apply configMap and secret.

### 1. Create namespace:
    kubectl apply -f .infrastructure/namespace.yml

### 2. Apply configMap:
    kubectl apply -f .infrastructure/confgiMap.yml -n todoapp

### 3. Apply Secret:
    kubectl apply -f .infrastructure/secret.yml -n todoapp

### 4. Update deployment:
    kubectl apply -f .infrastructure/deployment.yml -n todoapp

## How to validate changes.

### Pod validation:
    kubectl get pods -n todoapp
    POD=$(kubectl get pods -n todoapp -l app=todoapp -o jsonpath='{.items[0].metadata.name}')
    kubectl describe pod $POD -n todoapp
    kubectl exec -n todoapp $POD -- printenv | grep SECRET_KEY
    kubectl exec -n todoapp $POD -- printenv | grep PYTHONUNBUFFERED

### 1. Check configMap:
    kubectl get confgimap -n todoapp

### 2. Check Secret:
    kubectl get secret app-secret -n todoapp
    kubectl get secret app-secret -n todoapp -o jsonpath='{.data.SECRET_KEY}'