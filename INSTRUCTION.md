# створити namespace (якщо ще не існує)
kubectl create namespace mateapp || true

# застосувати ConfigMap
kubectl -n mateapp apply -f .infrastructure/configmap.yml

# застосувати Secret
kubectl -n mateapp apply -f .infrastructure/secret.yml

# застосувати Deployment (оновлений, з env з ConfigMap і Secret)
kubectl -n mateapp apply -f .infrastructure/deployment.yml
