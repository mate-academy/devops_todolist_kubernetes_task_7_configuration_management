1) Apply the manifests

# Apply namespace, ConfigMap, Secret and Deployment
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/confgiMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

2) Validate resources were created

# List namespaces (if using default, omit -n)
kubectl get ns

# Check ConfigMap
kubectl get configmap
kubectl describe configmap <configmap-name>

# Check Secret
kubectl get secrets
kubectl secret <secret-name>
kubectl get secret <secret-name> -o jsonpath=’{.data.*}’

# Check Deployment/Pod
kubectl get deployments
kubectl get pods # todoapp pods should be running
kubectl describe deployment <deployment-name>

3) Verify environment variables in Pod
kubectl exec -it <todoapp-pod-name> -- printenv PYTHONUNBUFFERED
kubectl exec -it <todoapp-pod-name> -- printenv SECRET_KEY

4) Test web app

# Port-forward to access the app locally (adjust service or pod name and ports if different)
kubectl port-forward svc/<service-name> 8000:8080

# Then open http://localhost:8000 in your browser or use curl
curl -I http://localhost:8000