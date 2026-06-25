
# Apply the ConfigMap
kubectl apply -f configMap.yml -n mateapp

# Apply the Secret
kubectl apply -f secret.yml -n mateapp

# Apply the Deployment
kubectl apply -f deployment.yml -n mateapp

# Verify resources are created
kubectl get configmap -n mateapp
kubectl get secret -n mateapp
kubectl get deployment -n mateapp
kubectl get pods -n mateapp

# --- ConfigMap Validation ---

# Check that ConfigMap exists
kubectl get configmap app-config -n mateapp

# Describe the ConfigMap to see its contents
kubectl describe configmap app-config -n mateapp

# Verify that the key PYTHONUNBUFFERED exists and has value "1"
kubectl get configmap app-config -n mateapp -o jsonpath='{.data.PYTHONUNBUFFERED}'

# Check that Deployment uses the ConfigMap for environment variable
kubectl describe deployment <deployment-name> -n todoapp | grep -A 2 PYTHONUNBUFFERED

# Verify inside a running Pod that the environment variable is set
kubectl exec -n mateapp -it <pod-name> -- env | grep PYTHONUNBUFFERED

# Expected output: "1"


# --- Secret Validation ---

# Check that Secret exists
kubectl get secret app-secrets -n mateapp

# Describe Secret (does not reveal actual value)
kubectl describe secret app-secrets -n mateapp

# Verify the SECRET_KEY exists in Secret
kubectl get secret app-secrets -n mateapp -o jsonpath='{.data.SECRET_KEY}'

# Check that Deployment uses the Secret for environment variable
kubectl describe deployment <deployment-name> -n todoapp | grep -A 2 SECRET_KEY

# Verify inside a running Pod that the SECRET_KEY environment variable is set
kubectl exec -n mateapp -it <pod-name> -- env | grep SECRET_KEY

# Expected output: the base64-decoded secret key, e.g. "super-secret-key"

# Optional: Decode the base64 SECRET_KEY to verify it matches the intended value
kubectl get secret app-secrets -n mateapp -o jsonpath='{.data.SECRET_KEY}' | base64 --decode; echo
