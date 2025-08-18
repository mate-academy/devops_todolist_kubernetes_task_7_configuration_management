kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

kubectl get secret todoapp-secret -o jsonpath=’{.data.*}’

kubectl get pods -n mateapp
kubectl exec -it todoapp-5bbfb5c7-5l9wf -n mateapp -- printenv | grep SECRET_KEY
kubectl exec -it todoapp-5bbfb5c7-5l9wf -n mateapp -- printenv | grep PYTHONUNBUFFERED