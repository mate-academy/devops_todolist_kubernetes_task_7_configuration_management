# Configuration Management

## Prerequisites
* Namespace `mateapp`

### Step 0: Convert to base64 SECRET_KEY value from the settings.py:

```bash
[convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes("<SECRET_KEY value>"))
```

### Step 1: Apply all manifests to the mateapp namespace:

```bash
# Deploy ConfigMap
kubectl apply -f .infrastructure/configMap.yml

# Deploy Secrets
kubectl apply -f .infrastructure/secret.yml

# Deploy todoapp application
kubectl apply -f .infrastructure/deployment.yml
```

### Step 2: Confirm all resources are running in mateapp namespace:

```bash
# Check ConfigMap
kubectl get configmap -n mateapp

# Check Secrets
kubectl get secrets -n mateapp

# Check Deployment
kubectl get deployment -n mateapp
```

### Step 3: Verify and validate the deployment works in mateapp namespace:

```bash
# Validate deployment
kubectl port-forward deployment/todoapp 8083:8080 -n mateapp
```

To make sure todoapp is working use this link: http://localhost:8083/.

```bash
# Check pods
kubectl get pods -n mateapp

# Verify the container received the environment variables as SECRET_KEY and PYTHONUNBUFFERED via connect to the todoapp application and execute 'printenv' command inside:
kubectl exec -it <pod-name> -n mateapp -- sh
```