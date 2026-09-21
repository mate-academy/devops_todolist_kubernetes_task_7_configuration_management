# Configuration Management Instructions

## ConfigMap

Apply ConfigMap:
kubectl apply -f .infrastructure/configMap.yml

Check ConfigMap:
kubectl get configmaps -n todoapp

## Secret

Apply Secret:
kubectl apply -f .infrastructure/secret.yml

Check Secret:
kubectl get secrets -n todoapp

## Deployment

Apply Deployment:
kubectl apply -f .infrastructure/deployment.yml

Check Pods:
kubectl get pods -n todoapp

## Check Environment Variables

Check PYTHONUNBUFFERED:
kubectl exec -it <pod-name> -n todoapp -- printenv PYTHONUNBUFFERED

Check SECRET_KEY:
kubectl exec -it <pod-name> -n todoapp -- printenv SECRET_KEY
