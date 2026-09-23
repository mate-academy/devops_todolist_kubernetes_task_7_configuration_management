# INSTRUCTION.md

## Apply

kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

## Validate

kubectl get configmap -n todoapp
kubectl get secret -n todoapp
kubectl get deployment -n todoapp
kubectl get pods -n todoapp

kubectl exec -n todoapp <pod-name> -- printenv PYTHONUNBUFFERED
kubectl exec -n todoapp <pod-name> -- printenv SECRET_KEY

kubectl describe deployment todoapp -n todoapp