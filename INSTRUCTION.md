# Instruction

Commands to apply ConfigMap and Secret changes for the ToDo app, and how to validate them.

## Apply changes

```
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

## Validate changes

1. Check that the ConfigMap and Secret were created in the `todoapp` namespace:

```
kubectl get configmap custom-config -n todoapp
kubectl get secret custom-secrets -n todoapp
```

2. Check that the pod is up and running:

```
kubectl get pods -n todoapp
```

3. Check that the environment variables were injected into the container from the ConfigMap and Secret:

```
kubectl exec -n todoapp deploy/todoapp -- printenv PYTHONUNBUFFERED
kubectl exec -n todoapp deploy/todoapp -- printenv SECRET_KEY
```

`PYTHONUNBUFFERED` should print `1`, and `SECRET_KEY` should print the decoded value stored in the Secret.

4. Check that the application starts without errors (no `KeyError: SECRET_KEY`):

```
kubectl logs -n todoapp deploy/todoapp
```
