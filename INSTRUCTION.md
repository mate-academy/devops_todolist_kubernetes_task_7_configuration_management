# Instructions

## Prerequisites

Make sure that Kubernetes is running and `kubectl` is configured.

Check the cluster:

```
kubectl get nodes
```

Check the `todoapp` namespace:

```
kubectl get namespace todoapp
```

If the namespace does not exist:

```
kubectl create namespace todoapp
```

## Apply ConfigMap

Apply the ConfigMap:

```
kubectl apply -f .infrastructure/configMap.yml
```

Verify the ConfigMap:

```
kubectl get configmap todoapp-config -n todoapp
```

Check its configuration:

```
kubectl describe configmap todoapp-config -n todoapp
```

The ConfigMap must contain the `PYTHONUNBUFFERED` key.

## Apply Secret

Apply the Secret:

```
kubectl apply -f .infrastructure/secret.yml
```

Verify the Secret:

```
kubectl get secret todoapp-secret -n todoapp
```

Check the Secret metadata:

```
kubectl describe secret todoapp-secret -n todoapp
```

The Secret must contain the `SECRET_KEY` key.

## Deploy the ToDo application

Apply the Deployment:

```
kubectl apply -f .infrastructure/deployment.yml
```

Check the Deployment:

```
kubectl get deployment todoapp -n todoapp
```

Check the application Pods:

```
kubectl get pods -n todoapp
```

## Validate ConfigMap environment variable

Find the ToDo application Pod:

```
kubectl get pods -n todoapp
```

Check the `PYTHONUNBUFFERED` environment variable:

```
kubectl exec -n todoapp <pod-name> -- printenv PYTHONUNBUFFERED
```

The expected value is:

```
1
```

## Validate Secret environment variable

Check the `SECRET_KEY` environment variable:

```
kubectl exec -n todoapp <pod-name> -- printenv SECRET_KEY
```

The command should return the value provided by the Kubernetes Secret.

The application must use this environment variable instead of a hardcoded `SECRET_KEY` in `settings.py`.

## Validate Deployment configuration

Check the Deployment:

```
kubectl describe deployment todoapp -n todoapp
```

Verify that:

* `PYTHONUNBUFFERED` uses `configMapKeyRef`
* `SECRET_KEY` uses `secretKeyRef`
* the container image is `ikulyk404/todoapp:3.0.0`
* the container exposes port `8080`

You can also inspect the complete Deployment:

```
kubectl get deployment todoapp -n todoapp -o yaml
```

## Validate application logs

Check the application logs:

```
kubectl logs -n todoapp <pod-name>
```

The application should start successfully without a missing `SECRET_KEY` configuration error.

## Validate health endpoints

Check the application Service:

```
kubectl get svc -n todoapp
```

The application should expose the `/api/health` and `/api/ready` endpoints.

Check the Pod status:

```
kubectl get pods -n todoapp
```

The application Pod should become `Ready`.

## Validate all resources

```
kubectl get all -n todoapp
```

The ConfigMap, Secret, Deployment and application Pod should be present in the `todoapp` namespace.

## Cleanup

Remove the Deployment:

```
kubectl delete -f .infrastructure/deployment.yml
```

Remove the ConfigMap:

```
kubectl delete -f .infrastructure/configMap.yml
```

Remove the Secret:

```
kubectl delete -f .infrastructure/secret.yml
```
