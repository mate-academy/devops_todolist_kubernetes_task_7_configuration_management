# How to apply ConfigMap & Secret and validate configuration

## 1. Build and push Docker image (one time, if not built yet)

From the `src` folder you can build and push the image to Docker Hub:

```bash
cd src

# Build image for Django Todo application
docker build -t ikulyk404/todoapp:3.0.0 .

# Push image to Docker Hub
docker push ikulyk404/todoapp:3.0.0

# Go back to repository root
cd ..

# 1) Namespace
kubectl apply -f _infrastructure/namespace.yml

# 2) Existing objects used by previous tasks (optional, but useful for testing)
kubectl apply -f _infrastructure/clusterIp.yml
kubectl apply -f _infrastructure/nodeport.yml
kubectl apply -f _infrastructure/busybox.yml

# 3) New configuration objects for this task
kubectl apply -f _infrastructure/configmap.yml
kubectl apply -f _infrastructure/secret.yml

# 4) Deployment and HPA (if needed)
kubectl apply -f _infrastructure/deployment.yml
kubectl apply -f _infrastructure/hpa.yml

# Check namespace resources
kubectl get all -n todoapp

# Check ConfigMap
kubectl get configmap todoapp-config -n todoapp
kubectl describe configmap todoapp-config -n todoapp

# Check Secret (only metadata is visible)
kubectl get secret todoapp-secret -n todoapp
kubectl describe secret todoapp-secret -n todoapp

# Check Deployment and Pods
kubectl get deploy -n todoapp
kubectl get pods -n todoapp

# Validate ConfigMap usage (PYTHONUNBUFFERED)
# Get one of the running pods
POD_NAME=$(kubectl get pods -n todoapp -l app=todoapp -o jsonpath='{.items[0].metadata.name}')

# Print environment variables inside the pod and filter by PYTHONUNBUFFERED
kubectl exec -n todoapp "$POD_NAME" -- env | grep PYTHONUNBUFFERED

# Validate Secret usage (SECRET_KEY)
# Reuse the same POD_NAME (or compute again if needed)
POD_NAME=$(kubectl get pods -n todoapp -l app=todoapp -o jsonpath='{.items[0].metadata.name}')

# Print only SECRET_KEY variable
kubectl exec -n todoapp "$POD_NAME" -- env | grep SECRET_KEY

# Access the application via NodePort
# Get node IP
kubectl get nodes -o wide

# Suppose the node internal IP is 10.0.0.10 and NodePort is 30007:
# Then open:
http://10.0.0.10:30007/

# Cleanup
kubectl delete -f _infrastructure/deployment.yml
kubectl delete -f _infrastructure/hpa.yml
kubectl delete -f _infrastructure/nodeport.yml
kubectl delete -f _infrastructure/clusterIp.yml
kubectl delete -f _infrastructure/busybox.yml
kubectl delete -f _infrastructure/secret.yml
kubectl delete -f _infrastructure/configmap.yml
kubectl delete -f _infrastructure/namespace.yml