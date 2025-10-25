Apply configuration:

kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/confgiMap.yml

Check that Secret and ConfigMap are applied:
kubectl get secret -n <namespace>
kubectl get configmap -n <namespace>

Check pods have updated environment
kubectl exec -it <pod-name> -- printenv | grep SECRET_KEY