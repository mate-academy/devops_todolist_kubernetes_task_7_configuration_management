Apply ConfigMap

The configMap.yml file contains the PYTHONUNBUFFERED environment variable.

Apply ConfigMap:

kubectl apply -f .infrastructure/configMap.yml

Verify ConfigMap:

kubectl get configmap todoapp-config -n todoapp
kubectl describe configmap todoapp-config -n todoapp

Expected value:

PYTHONUNBUFFERED: 1

Apply Secret

kubectl apply -f .infrastructure/secret.yml

Verify that the Secret exists:

kubectl get secret todoapp-secret -n todoapp

Check the Secret keys without displaying their values:

kubectl describe secret todoapp-secret -n todoapp

Apply Deployment

The Deployment uses ConfigMap and Secret through environment variables:

PYTHONUNBUFFERED — loaded from ConfigMap todoapp-config.

SECRET_KEY — loaded from Secret todoapp-secret.

Apply the updated Deployment:

kubectl apply -f .infrastructure/deployment.yml

Check the rollout status:

kubectl rollout status deployment/todoapp -n todoapp

Validate Changes

Check all resources:

kubectl get configmaps -n todoapp
kubectl get secrets -n todoapp
kubectl get deployments -n todoapp
kubectl get pods -n todoapp
