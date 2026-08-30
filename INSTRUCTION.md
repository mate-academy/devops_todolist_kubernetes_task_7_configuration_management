### 1. Create namespace: 
`kubectl apply -f .infrastructure/namespace.yml`

### 2. Apply the ConfigMap to set the PYTHONUNBUFFERED environment variable:
`kubectl apply -f .infrastructure/configMap.yml`

### 3. Apply the Secret so the password isn't hardcoded:
`kubectl apply -f .infrastructure/secret.yml`

### 4. Apply the Deployment:
`kubectl apply -f .infrastructure/deployment.yml`

### 5. Checking password creation and decoding it:
`kubectl get secret secret-mateapp -o jsonpath='{.data.*}' -n mateapp | base64 -d`

### 6. Check whether the deployment's pod gets the password and the ConfigMap value:
 - checking whether pods were created:
 `kubectl get pods -n mateapp`
 - checking whether the pod has the env vars:
 `kubectl exec -n mateapp <pod-name> -- env | grep -E "SECRET_KEY|PYTHONUNBUFFERED"`
 *If it prints the decoded key value and `PYTHONUNBUFFERED=1`, the Deployment successfully picked up both the Secret and the ConfigMap.*

### 7. Checking pods health and readiness
`kubectl exec -n mateapp <pod-name> -- curl -s http://localhost:8080/api/health`
`kubectl exec -n mateapp <pod-name> -- curl -s http://localhost:8080/api/ready`


