## Етап 1: Застосування ресурсів Kubernetes

Застосуйте всі три маніфести

1.  **Створити Secret** (todoapp-secrets):
    ```bash
    kubectl apply -f secret.yml --namespace todoapp
    ```

2.  **Створити ConfigMap** (todoapp-config-map):
    ```bash
    kubectl apply -f configMap.yml --namespace todoapp
    ```

3.  **Розгорнути застосунок** (Deployment):
    ```bash
    kubectl apply -f deployment.yml --namespace todoapp
    ```

## Етап 2: Валідація Змін

Валідація підтверджує, що змінні оточення коректно інжектовані всередину Pod'а.

1.  **Перевірка статусу Pod'ів:**
    Переконайтеся, що Deployment створив робочий Pod:
    ```bash
    kubectl get pods -l app=todoapp --namespace todoapp
    ```

2.  **Валідація інжектованих змінних:**
    Перевірте, що змінні оточення коректно присутні всередині контейнера.

    ```bash
    # 1. Отримайте ім'я Pod'а:
    export TODO_POD=$(kubectl get pods -l app=todoapp --namespace todoapp -o jsonpath='{.items[0].metadata.name}')

    # 2. Валідація PYTHONUNBUFFERED (з ConfigMap):
    echo "--- ConfigMap (PYTHONUNBUFFERED) ---"
    kubectl exec $TODO_POD --namespace todoapp -- printenv | grep PYTHONUNBUFFERED
    # Очікуваний результат: PYTHONUNBUFFERED=1

    # 3. Валідація SECRET_KEY (з Secret):
    echo "--- Secret (SECRET_KEY) ---"
    kubectl exec $TODO_POD --namespace todoapp -- printenv | grep SECRET_KEY
    # Очікуваний результат: SECRET_KEY=helloharry
    ```