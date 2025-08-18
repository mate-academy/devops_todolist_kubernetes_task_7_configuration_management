kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

kubectl get secret todoapp-secrets -o jsonpath=’{.data.*}’