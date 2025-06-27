# Інструкції з розгортання Django ToDo App у Kubernetes

Цей документ містить покрокові інструкції щодо розгортання Django ToDo List додатка в кластері Kubernetes, використовуючи ConfigMap та Secret.

## Передумови

Перед початком переконайтеся, що у вас встановлено та налаштовано наступне:

1.  **Docker:** Для збірки образу додатка.
2.  **kubectl:** Інструмент командного рядка для взаємодії з кластером Kubernetes.
3.  **Доступ до кластера Kubernetes:** (наприклад, Minikube, Docker Desktop з увімкненим Kubernetes, або хмарний кластер).
4.  **Репозиторій Docker:** (наприклад, Docker Hub) для зберігання вашого Docker образу.
5.  **Згенерований `SECRET_KEY`:** Ви повинні були згенерувати код за допомогою команди `python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'` 
6.  **Закодувати `SECRET_KEY`:** Згенерований код закодувати за допомогою команди `echo -n 'SECRET_KEY' | base64`, де SECRET_KEY - згенерований у попередній команді код.

## Етапи розгортання

Виконайте наступні кроки:

### Крок 1: Форкніть репозиторій та внесіть зміни у `settings.py`

1.  **Форкніть цей репозиторій** до свого облікового запису GitHub.
2.  **Клонуйте** форкнутий репозиторій на свою локальну машину.
3.  **Змініть файл `settings.py`** у своєму локальному репозиторії. Знайдіть рядок `SECRET_KEY = "..."` і замініть його на:
    ```python
    import os
    SECRET_KEY = os.environ.get("SECRET_KEY", "simple-secret")
    ```
    Збережіть зміни.

### Крок 2: Створіть Docker образ та завантажте його

1.  Перейдіть до кореневого каталогу вашого проекту Django, де знаходиться `Dockerfile`.
2.  Зберіть Docker образ:
    ```bash
    docker build -t leoleiden/todo-app:latest .
    ```

3.  Увійдіть до свого Docker реєстру (якщо ви ще цього не зробили):
    ```bash
    docker login
    ```
4.  Завантажте образ до реєстру:
    ```bash
    docker push leoleiden/todo-app:latest
    ```

### Крок 3: Створіть Kubernetes маніфести

1.  Створіть файл `confgiMap.yml` з наступним вмістом:
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: todo-app-config
    data:
      PYTHONUNBUFFERED: "1"
    ```
2.  Створіть файл `secret.yml` з наступним вмістом. **Не забудьте закодувати ваш `SECRET_KEY` в Base64!**
    ```yaml
    apiVersion: v1
    kind: Secret
    metadata:
      name: todo-app-secret
    type: Opaque
    data:
      SECRET_KEY: <SECRET_KEY_IN_BASE64>
    ```
3.  Створіть файл `deployment.yml` з наступним вмістом.
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: todo-app-deployment
      labels:
        app: todo-app
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: todo-app
      template:
        metadata:
          labels:
            app: todo-app
        spec:
          containers:
          - name: todo-app-container
            image: leoleiden/todo-app:latest
            ports:
            - containerPort: 8080
            envFrom:
            - configMapRef:
                name: todo-app-config
            env:
            - name: SECRET_KEY
              valueFrom:
                secretKeyRef:
                  name: todo-app-secret
                  key: SECRET_KEY
    ```

### Крок 4: Застосуйте маніфести до кластера Kubernetes

Переконайтеся, що ви перебуваєте в каталозі, де знаходяться ваші `.yml` файли.

1.  Застосуйте ConfigMap:
    ```bash
    kubectl apply -f configMap.yml
    ```
2.  Застосуйте Secret:
    ```bash
    kubectl apply -f secret.yml
    ```
3.  Застосуйте Deployment:
    ```bash
    kubectl apply -f deployment.yml
    ```

### Крок 5: Перевірка змін

1.  **Перевірте статус Pod'ів:** Переконайтеся, що ваш Pod успішно розгорнуто та запущено.
    ```bash
    kubectl get pods -l app=todo-app
    ```
    Ви повинні побачити Pod зі статусом `Running`:
    ```
    NAME                                  READY   STATUS    RESTARTS   AGE
    todo-app-deployment-8b84767bb-h5hx9   1/1     Running   0          21s
    ```
2.  **Перевірте логи Pod'у:** Переконайтеся, що додаток Django запускається без помилок і що `PYTHONUNBUFFERED` працює належним чином.
    ```bash
    kubectl logs -f <your-pod-name>
    ```
    *Замініть `<your-pod-name>` на ім'я вашого Pod'у з виводу команди `kubectl get pods`.*
    ```
    $ kubectl logs -f todo-app-deployment-8b84767bb-h5hx9
    Watching for file changes with StatReloader
    Performing system checks...

    System check identified no issues (0 silenced).
    June 27, 2025 - 10:51:09
    Django version 4.1.10, using settings 'todolist.settings'
    Starting development server at http://0.0.0.0:8080/
    Quit the server with CONTROL-C.
    
    ```
3.  **Перевірте змінні середовища в Pod'і:** Щоб переконатися, що `SECRET_KEY` та `PYTHONUNBUFFERED` правильно передаються до контейнера, ви можете виконати команду в Pod'і:
    ```bash
    kubectl exec -it <your-pod-name> -- printenv
    ```
    Ви повинні побачити `SECRET_KEY` та `PYTHONUNBUFFERED=1` у виводі. Наприклад, у моєму випадку:
```
$     kubectl exec -it todo-app-deployment-8b84767bb-h5hx9 -- printenv
PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=todo-app-deployment-8b84767bb-h5hx9
TERM=xterm
PYTHONUNBUFFERED=1
SECRET_KEY=2&kan6loe&(om1r173z_3q6=8l&re24o@ransob+5u&bvb_f=z
TODOLIST_NODEPORT_SERVICE_SERVICE_PORT=80
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
TODOLIST_CLUSTERIP_SERVICE_PORT=tcp://10.110.158.202:80
TODOLIST_CLUSTERIP_SERVICE_PORT_80_TCP_PORT=80
TODOLIST_CLUSTERIP_SERVICE_PORT_80_TCP_ADDR=10.110.158.202
TODOLIST_NODEPORT_SERVICE_PORT_80_TCP=tcp://10.103.187.178:80
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
TODOLIST_CLUSTERIP_SERVICE_SERVICE_PORT=80
TODOLIST_CLUSTERIP_SERVICE_PORT_80_TCP_PROTO=tcp
TODOLIST_NODEPORT_SERVICE_SERVICE_HOST=10.103.187.178
TODOLIST_NODEPORT_SERVICE_PORT_80_TCP_PORT=80
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PORT=443
TODOLIST_CLUSTERIP_SERVICE_SERVICE_HOST=10.110.158.202
TODOLIST_CLUSTERIP_SERVICE_PORT_80_TCP=tcp://10.110.158.202:80
TODOLIST_NODEPORT_SERVICE_PORT=tcp://10.103.187.178:80
TODOLIST_NODEPORT_SERVICE_PORT_80_TCP_PROTO=tcp
TODOLIST_NODEPORT_SERVICE_PORT_80_TCP_ADDR=10.103.187.178
KUBERNETES_SERVICE_PORT=443
LANG=C.UTF-8
GPG_KEY=E3FF2839C048B25C084DEBE9B26995E310250568
PYTHON_VERSION=3.8.20
HOME=/root
```

---
