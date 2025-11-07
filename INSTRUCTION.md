# Створи ресурси у Kubernetes:
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

# Перевір створені ресурси
kubectl get configmap
kubectl get secret
kubectl get deployment
kubectl get pods

# Перевір змінні середовища в поді
# Отримай ім’я поду:
kubectl get pods

# Перевір, що змінні застосувались:
kubectl exec -it <pod-name> -- printenv | grep -E 'PYTHONUNBUFFERED|SECRET_KEY'

# Перевір роботу застосунку
# Проглянь логи:
kubectl logs <pod-name>

# Або пробрось порт:
kubectl port-forward <pod-name> 8000:8000

# Відкрий у браузері:
http://localhost:8000/

# Очистка після перевірки
kubectl delete -f .infrastructure/deployment.yml
kubectl delete -f .infrastructure/configMap.yml
kubectl delete -f .infrastructure/secret.yml