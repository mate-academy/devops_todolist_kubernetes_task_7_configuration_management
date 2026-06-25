 # Instructions

## Deploy manifests

kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

## Validate the changes

# Pod should be Running and READY 1/1
kubectl get pods -n todoapp

# Check that env variables from ConfigMap and Secret are injected
kubectl exec -it -n todoapp deploy/todoapp -- env | grep -E "SECRET_KEY|PYTHONUNBUFFERED"