# Інструкція з розгортання та перевірки конфігурацій (ConfigMap & Secret)

##  1. Застосування змін у кластері

Перед запуском переконайтеся, що ви знаходитесь у правильному контексті Kubernetes та у вас створено простір імен (Namespace) `todoapp`.

Виконайте наступні команди по черзі, щоб застосувати конфігурації та оновити розгортання:

# 1. Створення ConfigMap
kubectl apply -f configMap.yml

# 2. Створення Secret
kubectl apply -f secret.yml

# 3. Запуск або оновлення Deployment
kubectl apply -f .infrastructure/deployment.yml

Команди для валідації та перевірка змін
# 4. Перевірити що ConfigMap застосовано
kubectl get configmap -n todoapp
# Перевірити що Secret застосовано
kubectl get secret -n todoapp
# Перевірити що под працює
kubectl get pods -n todoapp
# Перевірити змінні середовища в поді
kubectl exec -it <pod-name> -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"