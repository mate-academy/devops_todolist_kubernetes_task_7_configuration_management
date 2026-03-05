Дана інструкція описує процес розгортання та валідації конфігурацій для `todoapp`.

## 1. Розгортання (Deployment)

Перед запуском деплойменту необхідно створити неймспейс та застосувати ресурси конфігурації.

```powershell
# Створення неймспейсу (якщо не існує)
kubectl create namespace todoapp

# Застосування конфігурацій та секретів
kubectl apply -f configMap.yml
kubectl apply -f secret.yml

# Розгортання додатка
kubectl apply -f deployment.yml
```

## 2. Валідація змін

Щоб переконатися, що додаток правильно підтягнув змінні оточення, виконайте наступні кроки:

### Перевірка змінних оточення в Поді:

Знайдіть назву поду та виведіть його змінні оточення:

```powershell
# Отримання назви поду
$POD_NAME = (kubectl get pods -n todoapp -l app=todoapp -o jsonpath='{.items[0].metadata.name}')

# Перевірка PYTHONUNBUFFERED
kubectl exec $POD_NAME -n todoapp -- printenv PYTHONUNBUFFERED

# Перевірка наявності SECRET_KEY (не виводьте значення в публічні логи!)
kubectl exec $POD_NAME -n todoapp -- printenv SECRET_KEY

```

### Перевірка логів додатка:

Якщо `PYTHONUNBUFFERED` встановлено в `1`, логи Flask будуть з'являтися в консолі миттєво без затримок.

```powershell
kubectl logs -f $POD_NAME -n todoapp
```

### Перевірка стану (Probes):

Переконайтеся, що поди пройшли перевірку готовності:

```powershell
kubectl get pods -n todoapp
```

Статус має бути `Running`, а в колонці `READY` має бути `1/1`.

