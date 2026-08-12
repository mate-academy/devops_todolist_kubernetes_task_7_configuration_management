Застосування

kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

Перевірка

kubectl get configmap -n todoapp
kubectl get secret -n todoapp
kubectl get pods -n todoapp

kubectl exec -n todoapp <pod-name> -- env | grep PYTHONUNBUFFERED
kubectl exec -n todoapp <pod-name> -- env | grep SECRET_KEY

kubectl logs -n todoapp <pod-name>