# Інструкція з розгортання ToDo застосунку з використанням ConfigMap та Secret

1. Створення простору імен

kubectl apply -f namespace.yml
2. Створення ConfigMap

kubectl apply -f configMap.yml
3. Створення Secret

kubectl apply -f secret.yml
4. Створення Deployment

kubectl apply -f deployment.yml

5. Перевірка результатів

Перевірка встановлених змінних середовища

kubectl exec -it <one-of-the-pods> -n todoapp -- printenv | grep PYTHONUNBUFFERED
kubectl exec -it <one-of-the-pods> -n todoapp -- printenv | grep SECRET_KEY
Очікувано виведе:

PYTHONUNBUFFERED=1
SECRET_KEY=super-secret-key
Пояснення
ConfigMap використовується для зберігання конфігураційних змінних, які не є секретами.

Secret застосовується для зберігання чутливих даних, таких як ключі, токени або паролі.

Змінні підключені до контейнера через env блок у deployment.yml.