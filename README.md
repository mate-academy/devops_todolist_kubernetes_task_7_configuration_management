# Django ToDo List Kubernetes Deployment

This guide provides step-by-step instructions for deploying the Django ToDo list application on a Kubernetes cluster. This includes setting up ConfigMaps, Secrets, and a Deployment to manage the application environment.

## Prerequisites

- Kubernetes cluster
- kubectl configured to interact with your cluster
- Docker image of the Django ToDo list application

## Setup Instructions

### Step 1: Namespace Creation

Ensure that the `todoapp` namespace exists. If not, create it using the following command:

```bash
kubectl create namespace todoapp
```

### Step 2: Creating ConfigMap

The ConfigMap stores the environment configuration for the Django application.

1. Create a configMap.yml file with the following content:

```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: todoapp-config
  namespace: todoapp
data:
  PYTHONUNBUFFERED: "1"
```

2. Apply the ConfigMap using the following command:
```bash
kubectl apply -f configMap.yml
```

### Step 3: Creating Secret

The Secret will store sensitive information, like the Django SECRET_KEY.

1. Create a secret.yml file with the following content:

```bash
apiVersion: v1
kind: Secret
metadata:
  name: todoapp-secrets
  namespace: todoapp
type: Opaque
data:
  SECRET_KEY: <base64-secret-key>
```

Replace <base64-secret-key> with your base64 encoded Django SECRET_KEY.

Use the command on your terminal:
```bash
echo -n "your-secret-key" | base64
```

2.Apply the Secret using the following command:

```bash
kubectl apply -f secret.yml
```

### Step 4: Deploying the Application

1. Ensure the deployment.yml file includes the environment variables from the ConfigMap and Secret.

The env section of your containers should look like this:
```bash
env:
  - name: "PYTHONUNBUFFERED"
    valueFrom:
      configMapKeyRef:
        name: todoapp-config
        key: PYTHONUNBUFFERED
  - name: "SECRET_KEY"
    valueFrom:
      secretKeyRef:
        name: todoapp-secret
        key: SECRET_KEY
```

2. Apply the deployment using the following command:

```bash
kubectl apply -f deployment.yml
```

### Validation Instructions
To ensure that your application is running correctly:

1. Check the Deployment status:

```bash
kubectl -n todoapp get deployments
```

2. Verify the Pods are running:

```bash
kubectl -n todoapp get pods
```

3. Check the application logs:

```bash
kubectl -n todoapp logs <pod-name>
```

Replace <pod-name> with the name of your pod.

