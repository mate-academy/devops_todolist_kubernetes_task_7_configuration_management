In order to set up the secrets and the config map:

kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml

In order to validate that the environment variables are set correctly:

kubectl get pods
kubectl exec -it <pod-name> -- /bin/sh
prinenv