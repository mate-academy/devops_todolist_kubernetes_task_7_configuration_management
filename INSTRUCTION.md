# Инструкции по развертыванию Django ToDo приложения в Kubernetes

## Предварительные требования

1. Убедитесь, что у вас установлен и настроен Kubernetes кластер
2. Убедитесь, что у вас установлен `kubectl` и он настроен для работы с вашим кластером
3. Убедитесь, что Docker образ `todolist:latest` собран и доступен в кластере

## Сборка Docker образа

Если образ еще не собран, выполните следующие команды:

```bash
# Переходим в директорию с исходным кодом
cd src

# Собираем Docker образ
docker build -t todolist:latest .

# Если используете minikube, загружаем образ в minikube
minikube image load todolist:latest
```

## Применение Kubernetes манифестов

Выполните команды в следующем порядке:

### 1. Создание ConfigMap

```bash
kubectl apply -f confgiMap.yml
```

**Что происходит:** Создается ConfigMap с переменной окружения `PYTHONUNBUFFERED=1`, которая отключает буферизацию вывода Python.

### 2. Создание Secret

```bash
kubectl apply -f secret.yml
```

**Что происходит:** Создается Secret с закодированным в base64 секретным ключом Django `SECRET_KEY`.

### 3. Создание Deployment

```bash
kubectl apply -f deployment.yml
```

**Что происходит:** Создается Deployment, который:

- Запускает 1 реплику Django приложения
- Использует переменные из ConfigMap и Secret
- Настраивает ограничения ресурсов

### 4. Создание Service

```bash
kubectl apply -f service.yml
```

**Что происходит:** Создается Service для доступа к приложению внутри кластера.

## Валидация развертывания

### 1. Проверка статуса ресурсов

```bash
# Проверка ConfigMap
kubectl get configmap todolist-config -o yaml

# Проверка Secret
kubectl get secret todolist-secret -o yaml

# Проверка Deployment
kubectl get deployment todolist-deployment

# Проверка подов
kubectl get pods -l app=todolist

# Проверка Service
kubectl get service todolist-service
```

### 2. Проверка логов приложения

```bash
# Получение имени пода
kubectl get pods -l app=todolist

# Просмотр логов (замените <pod-name> на реальное имя пода)
kubectl logs <pod-name>
```

### 3. Проверка переменных окружения в поде

```bash
# Подключение к поду для проверки переменных окружения
kubectl exec -it <pod-name> -- env | grep -E "(PYTHONUNBUFFERED|SECRET_KEY)"
```

### 4. Тестирование приложения

```bash
# Port forwarding для доступа к приложению локально
kubectl port-forward service/todolist-service 8080:80

# Теперь приложение доступно по адресу http://localhost:8080
```

Откройте браузер и перейдите по адресу `http://localhost:8080` для проверки работы приложения.

### 5. Валидация использования SECRET_KEY из переменной окружения

**Критически важно:** Проверим, что приложение действительно использует SECRET_KEY из Secret, а не захардкоженное значение:

```bash
# Получаем имя пода
POD_NAME=$(kubectl get pods -l app=todolist -o jsonpath="{.items[0].metadata.name}")

# Проверяем текущее значение SECRET_KEY в поде
kubectl exec $POD_NAME -- printenv SECRET_KEY

# Изменяем Secret на новое значение (для теста)
# Сначала кодируем новый ключ в base64
NEW_SECRET_B64=$(echo -n "new-test-secret-key-12345" | base64)

# Обновляем Secret
kubectl patch secret todolist-secret -p='{"data":{"SECRET_KEY":"'$NEW_SECRET_B64'"}}'

# Перезапускаем Deployment для применения нового Secret
kubectl rollout restart deployment todolist-deployment

# Ждем завершения перезапуска
kubectl rollout status deployment todolist-deployment

# Получаем новое имя пода после перезапуска
POD_NAME=$(kubectl get pods -l app=todolist -o jsonpath="{.items[0].metadata.name}")

# Проверяем, что новое значение SECRET_KEY применилось
kubectl exec $POD_NAME -- printenv SECRET_KEY

# Проверяем логи приложения (должно работать с новым ключом)
kubectl logs $POD_NAME
```

**Ожидаемый результат:** 
- Приложение должно успешно перезапуститься с новым SECRET_KEY
- Команда `printenv SECRET_KEY` должна показать новое значение `new-test-secret-key-12345`
- Приложение должно работать корректно, что подтверждает отсутствие fallback на захардкоженное значение

**Возврат к исходному Secret:**
```bash
# Возвращаем исходное значение SECRET_KEY
kubectl patch secret todolist-secret -p='{"data":{"SECRET_KEY":"QGUyKHl4KXYmdGdoM19zPTB5amEtaSFkcGVieHN6XmRnNDd4KS1rJmtxXzN6Zio5ZSo="}}'

# Перезапускаем Deployment
kubectl rollout restart deployment todolist-deployment
```

## Ожидаемые результаты

1. **ConfigMap создан** - содержит `PYTHONUNBUFFERED=1`
2. **Secret создан** - содержит закодированный `SECRET_KEY`
3. **Pod запущен** - статус `Running`
4. **Переменные окружения настроены** - `PYTHONUNBUFFERED=1` и `SECRET_KEY` доступны в контейнере
5. **Приложение отвечает** - Django ToDo приложение доступно через Service

## Устранение проблем

### Если под не запускается:

```bash
# Проверка событий
kubectl describe pod <pod-name>

# Проверка логов
kubectl logs <pod-name>
```

### Если переменные окружения не установлены:

```bash
# Проверка ConfigMap
kubectl describe configmap todolist-config

# Проверка Secret
kubectl describe secret todolist-secret

# Проверка Deployment
kubectl describe deployment todolist-deployment
```

### Если образ не найден:

```bash
# Убедитесь, что образ собран и доступен
docker images | grep todolist

# Для minikube убедитесь, что образ загружен
minikube image ls | grep todolist
```

## Очистка ресурсов

Для удаления всех созданных ресурсов:

```bash
kubectl delete -f service.yml
kubectl delete -f deployment.yml
kubectl delete -f secret.yml
kubectl delete -f confgiMap.yml
```
