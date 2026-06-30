kubectl apply -f namespace.yml

kubectl config set-context --current --namespace=mateapp

kubectl apply -f configMap.yml

kubectl apply -f deployment.yml

kubectl get pods
copy <pod-name>

kubectl exec -it <pod-name> -- printenv | grep EXTERNAL_ENDPOINT

kubectl exec -it <pod-name> -- printenv | grep PYTHONUNBUFFERED

kubectl exec -it <pod-name> -- printenv | grep SECRET_KEY