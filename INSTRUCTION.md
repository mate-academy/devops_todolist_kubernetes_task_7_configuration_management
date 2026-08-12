# INSTRUCTION.md

Цей файл містить інструкції для застосування змін у Kubernetes кластері для ToDo застосунку.

---

## 🔹 1. Застосування ConfigMap

```bash
kubectl apply -f configMap.yml
🔹 2. Застосування Secret
```bash
kubectl apply -f secret.yml
🔹 3. Застосування Deployment
```bash
kubectl apply -f deployment.yml
🔹 4. Перевірка, що змінні середовища працюють
Подивитись список подів:

```bash

kubectl get pods -n todoapp
Підключитись до пода:

```bash
kubectl exec -n todoapp -it <pod-name> -- sh
Перевірити, чи змінні підставились:

```bash
printenv | grep SECRET_KEY
printenv | grep PYTHONUNBUFFERED
🔹 5. Перевірка готовності застосунку
Готовність:

```bash
curl http://<node-ip>:<nodeport>/api/ready
Живучість:

```bash
curl http://<node-ip>:<nodeport>/api/health
Заміни <node-ip> і <nodeport> на фактичні значення (залежить від сервісу).

🔹 6. Зміна settings.py
У src/todolist/settings.py:

```python
import os

SECRET_KEY = os.environ.get('SECRET_KEY', 'fallback-secret-key-for-dev')
