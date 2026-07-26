Apply manifests

kubectl apply -f .infrastructure/confgiMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml

How to validate the changes
1. Validate that the ConfigMap was created
kubectl get configmap
kubectl describe configmap todoapp-config

You should see the key PYTHONUNBUFFERED in the output.

2. Validate that the Secret was created
kubectl get secret
kubectl describe secret todoapp-secret

You should see the key SECRET_KEY (value will be hidden because it is base64-encoded).

To decode it:

kubectl get secret todoapp-secret -o jsonpath="{.data.SECRET_KEY}" | base64 --decode

3. Validate that the Deployment uses environment variables

Check pod environment variables:

kubectl get pods
kubectl exec -it <pod-name> -- env | grep PYTHONUNBUFFERED
kubectl exec -it <pod-name> -- env | grep SECRET_KEY