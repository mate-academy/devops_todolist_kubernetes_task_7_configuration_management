## How to apply configMap and secret.

### 1. Create namespace:
    kubectl apply -f .infrastructure/namespace.yml -n todoapp

### 2. Apply configMap:
    kubectl apply -f .infrastructure/configMap.yml -n todoapp

### 3. Update deployment:
    kubectl apply -f .infrastructure/deployment.yml -n todoapp

### 4. Apply Secret:
    kubectl apply -f .infrastructure/secret.yml -n todoapp

## How to validate changes.

### 1. Check configMap:
    kubectl get configmap

### 2. Check Secret:
    kubectl get secrets