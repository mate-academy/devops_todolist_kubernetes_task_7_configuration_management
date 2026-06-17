# Інструкції для розгортання Django додатку з ConfigMap та Secret

## Попередні вимоги

- Kubernetes кластер запущений
- `kubectl` встановлений та налаштований
- Docker образ `django-app:latest` завантажений у реєстр

## Застосування змін

### 1. Застосувати ConfigMap
```bash
kubectl apply -f configMap.yml
```

Очікуваний результат:
```
configmap/django-config created
```

### 2. Застосувати Secret
```bash
kubectl apply -f secret.yml
```

Очікуваний результат:
```
secret/django-secret created
```

### 3. Застосувати Deployment
```bash
kubectl apply -f deployment.yml
```

Очікуваний результат:
```
deployment.apps/django-app created
```

### 4. Застосувати все одночасно (альтернативно)
```bash
kubectl apply -f configMap.yml -f secret.yml -f deployment.yml
```

## Перевірка змін

### 1. Перевірити що ConfigMap створений
```bash
kubectl get configmap django-config
kubectl describe configmap django-config
```

Очікуваний результат - має показати `PYTHONUNBUFFERED: 1`

### 2. Перевірити що Secret створений
```bash
kubectl get secret django-secret
kubectl describe secret django-secret
```

Очікуваний результат - має показати `SECRET_KEY` в data секції

### 3. Перевірити що Deployment створений
```bash
kubectl get deployment django-app
kubectl describe deployment django-app
```

### 4. Перевірити що Pod запущений
```bash
kubectl get pods -l app=django-app
```

Чекайте доки Pod буде в стані `Running`:
```bash
kubectl get pods -l app=django-app -w
```

### 5. Отримати ім'я Pod
```bash
POD_NAME=$(kubectl get pods -l app=django-app -o jsonpath='{.items[0].metadata.name}')
echo $POD_NAME
```

### 6. Перевірити змінні оточення в Pod
```bash
kubectl exec -it $POD_NAME -- env | grep PYTHONUNBUFFERED
kubectl exec -it $POD_NAME -- env | grep SECRET_KEY
```

Очікуваний результат:
```
PYTHONUNBUFFERED=1
SECRET_KEY=django-insecure-key-change-in-production
```

### 7. Перевірити логи додатку
```bash
kubectl logs -f $POD_NAME
```

Переконайтеся що немає помилок пов'язаних з SECRET_KEY

### 8. Перевірити що Django використовує SECRET_KEY з оточення
```bash
kubectl exec -it $POD_NAME -- python manage.py shell
```

В Django shell виконайте:
```python
from django.conf import settings
print(f"SECRET_KEY: {settings.SECRET_KEY}")
# Має показати значення з Secret, не hardcoded значення
```

### 9. Перевірити що додаток працює
```bash
kubectl port-forward $POD_NAME 8000:8000
```

Тепер відкрийте браузер на `http://localhost:8000` або:
```bash
curl http://localhost:8000
```

## Оновлення Secret (якщо потрібно змінити SECRET_KEY)

### 1. Видалити старий Secret
```bash
kubectl delete secret django-secret
```

### 2. Оновити значення SECRET_KEY у файлі `secret.yml`

### 3. Застосувати новий Secret
```bash
kubectl apply -f secret.yml
```

### 4. Перезавантажити Pod щоб використав новий Secret
```bash
kubectl rollout restart deployment/django-app
```

### 5. Дочекатися що Pod перезапустився
```bash
kubectl get pods -l app=django-app -w
```

## Видалення всіх ресурсів

```bash
kubectl delete deployment django-app
kubectl delete configmap django-config
kubectl delete secret django-secret
```

## Моніторинг та налагодження

### Дивитися логи в реальному часі
```bash
kubectl logs -f deployment/django-app
```

### Отримати детальну інформацію про Pod
```bash
kubectl describe pod $POD_NAME
```

### Запустити команду всередині Pod
```bash
kubectl exec -it $POD_NAME -- /bin/bash
```

### Перевірити які значення використовує додаток
```bash
kubectl exec -it $POD_NAME -- python -c "import os; print('PYTHONUNBUFFERED:', os.getenv('PYTHONUNBUFFERED')); print('SECRET_KEY:', os.getenv('SECRET_KEY'))"
```

## Очікувані результати

✅ ConfigMap створений з `PYTHONUNBUFFERED=1`  
✅ Secret створений з `SECRET_KEY`  
✅ Deployment використовує обидва ресурси  
✅ Pod запущений без помилок  
✅ Django читає `SECRET_KEY` з оточення (з Secret)  
✅ Django читає `PYTHONUNBUFFERED` з оточення (з ConfigMap)  
✅ Додаток доступний і працює коректно  

## Важливо

- SECRET_KEY у `secret.yml` повинен бути base64 encoded
- Не забудьте змінити SECRET_KEY на production значення перед розгортанням
- Переконайтеся що Django `settings.py` читає SECRET_KEY з `os.getenv('SECRET_KEY')`
- Для production використовуйте дійсний Secret менеджер замість Kubernetes Secret