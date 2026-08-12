
4. Створіть INSTRUCTION.md
markdown
Copy
Edit
## Інструкції

### 1. Застосування змін

1. Створіть ConfigMap:
```bash
   kubectl apply -f .infrastructure/configMap.yml
```
   
2. Створіть Secret:
```bash
kubectl apply -f .infrastructure/secret.yml
```
3. Створіть Deployment:
```bash
kubectl apply -f .infrastructure/deployment.yml
```
## 2. Перевірка змін
1. Перевірте, чи ConfigMap застосовано:
```bash
kubectl get configmap todo-app-config -o yaml
```
2. Перевірте, чи Secret застосовано:
```bash
kubectl get secret todo-app-secret -o yaml
```
3. Перевірте, чи запущений под:
```bash
kubectl get pods
```
4. Перевірте змінні оточення у поді:
```bash
kubectl exec <pod-name> -- env | grep PYTHONUNBUFFERED
kubectl exec <pod-name> -- env | grep SECRET_KEY
```
Замініть <pod-name> на ім'я вашого пода.