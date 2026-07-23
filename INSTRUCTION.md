# ToDo App — ConfigMap & Secret Instructions

## Prerequisites

- A running Kubernetes cluster with the `todoapp` namespace and the ToDo app
  Deployment already created
- `kubectl` installed and configured

## 1. Apply the manifests

Apply the ConfigMap and Secret first, then the updated Deployment so it can
pick up the new environment variables:

```bash
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

## 2. Validate the changes

Check that the pods restarted with the new configuration:

```bash
kubectl get pods -n todoapp
```

Confirm the ConfigMap and Secret were created:

```bash
kubectl get configmap -n todoapp
kubectl get secret -n todoapp
```

Verify that the `PYTHONUNBUFFERED` and `SECRET_KEY` environment variables
are actually set inside a running pod:

```bash
kubectl exec -n todoapp <pod-name> -- env | grep PYTHONUNBUFFERED
kubectl exec -n todoapp <pod-name> -- env | grep SECRET_KEY
```

(Replace `<pod-name>` with an actual pod name from `kubectl get pods -n todoapp`.)

You can also confirm the values were pulled from the ConfigMap/Secret rather
than hardcoded, by describing the pod and checking its environment source:

```bash
kubectl describe pod <pod-name> -n todoapp
```

Look for the `Environment Variables from:` section, which should reference
the ConfigMap and Secret by name.

## Cleaning up

```bash
kubectl delete -f .infrastructure/secret.yml
kubectl delete -f .infrastructure/configMap.yml
```