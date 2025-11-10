Перевірка ConfigMap

Щоб перевірити ConfigMap, потрібно застосувати зміни:

kubectl apply -f configMap.yml
kubectl apply -f deployment.yml


Перевіряємо, що ConfigMap створено:

kubectl get configmap


Приклад виводу:

NAME               DATA   AGE
configmap-name     1      10s


Далі перевіряємо поди:

kubectl get pods


Щоб переконатися, що змінна PYTHONUNBUFFERED застосувалась у поді:

kubectl exec <pod-name> -- printenv | grep PYTHONUNBUFFERED

Перевірка Secret

Щоб перевірити Secret, застосовуємо зміни:

kubectl apply -f secret.yml
kubectl apply -f deployment.yml


Перевіряємо створений Secret:

kubectl get secret


І перевіряємо, що змінна SECRET_KEY доступна в контейнері:

kubectl exec <pod-name> -- printenv | grep SECRET_KEY