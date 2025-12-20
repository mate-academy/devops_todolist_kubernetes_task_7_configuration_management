# Інструкція з застосування ConfigMap та Secret і перевірки деплою

Команди (Windows cmd.exe). Виконуйте в корені проєкту:

1) Створити гілку для змін (локально):

```cmd
git checkout -b feature/configmap-secret
```

2) Створити namespace `todoapp`, якщо його немає:

```cmd
kubectl get namespace todoapp || kubectl create namespace todoapp
```

3) Застосувати ConfigMap, Secret і Deployment:

```cmd
kubectl apply -f .infrastructure/configMap.yml -n todoapp
kubectl apply -f .infrastructure/secret.yml -n todoapp
kubectl apply -f .infrastructure/deployment.yml -n todoapp
```

> Примітка: манифести в `.infrastructure` містять поле `namespace: todoapp`. Якщо ви не хочете вказувати `-n todoapp`, можна виконати `kubectl apply -f .infrastructure/`.

4) Перевірити успішний rollout Deployment:

```cmd
kubectl rollout status deployment/todoapp -n todoapp
```

5) Перевірки манифестів і ресурсів:

```cmd
kubectl get configmap todoapp-config -n todoapp -o yaml
kubectl get secret todoapp-secret -n todoapp -o yaml
kubectl describe deployment todoapp -n todoapp
kubectl get pods -n todoapp
```

6) Перевірити, що в запущеному контейнері присутні змінні оточення `PYTHONUNBUFFERED` і `SECRET_KEY`.
Спочатку отримайте ім'я пода (підставте у наступних командах):

```cmd
kubectl get pods -n todoapp
```

Потім (підставте <pod-name>):

```cmd
Windows:
kubectl exec -n todoapp <pod-name> -- printenv | findstr PYTHONUNBUFFERED
kubectl exec -n todoapp <pod-name> -- printenv | findstr SECRET_KEY

Linux/MacOS альтернатива:
kubectl exec -n todoapp <pod-name> -- printenv | grep PYTHONUNBUFFERED
kubectl exec -n todoapp <pod-name> -- printenv | grep SECRET_KEY

```

7) Переконатися, що `SECRET_KEY` не зашитий у `src/todolist/settings.py` (файл вже читає значення з оточення). Для швидкої перевірки:

```cmd
findstr /n "SECRET_KEY = os.getenv('SECRET_KEY')" src\todolist\settings.py
```

