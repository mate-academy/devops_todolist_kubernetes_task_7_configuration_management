ensure all changes applied and new pods created:

kubectl apply -f .infrastructure/confgiMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

check that key is retrievable via command below:

kubectl get secret todo-secret -o jsonpath='{.data.SECRET_KEY}'

access site and add a few tasks to ensure all works as expected:
http://localhost:30007/