# Застосувати всі маніфести:
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
# Перевірити стан:
kubectl get pods
kubectl get configmap
kubectl get secret