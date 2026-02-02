Додати ConfigMap та Secret, підключити їх до Deployment і перевірити, що змінні середовища використовуються застосунком.

Застосування

Створити ConfigMap:
kubectl apply -f configMap.yml

Створити Secret:
kubectl apply -f secret.yml

Застосувати Deployment:
kubectl apply -f deployment.yml

Перевірка

Перевірити, що ресурси створені:
kubectl get configmap -n todoapp
kubectl get secret -n todoapp
kubectl get pods -n todoapp

Перевірити змінні середовища в Pod:
kubectl exec -n todoapp <pod-name> -- env | grep PYTHONUNBUFFERED
kubectl exec -n todoapp <pod-name> -- env | grep SECRET_KEY

Перевірити логи застосунку:
kubectl logs -n todoapp <pod-name>