Застосувати всі YAML-файли (припускаю, що файли називаються secret.yaml, configmap.yaml, deployment.yaml):
kubectl apply -f secret.yml
kubectl apply -f configmap.yml
kubectl apply -f deployment.yml
	2.	Перевірити статус подів у namespace todoapp:
kubectl get pods -n todoapp
	3.	Перевірити, чи контейнер працює і всі контейнери готові:
kubectl describe pod <ім’я-пода> -n todoapp | grep -A5 “Containers:”
	4.	Перевірити змінні середовища SECRET_KEY і PYTHONUNBUFFERED у контейнері:
kubectl exec -n todoapp <ім’я-пода> – printenv | grep SECRET_KEY
kubectl exec -n todoapp <ім’я-пода> – printenv | grep PYTHONUNBUFFERED