# Django ToDoApp — Configuration Management

## Overview

This project deploys the Django ToDo application to Kubernetes and uses Kubernetes `ConfigMap` and `Secret` resources for application configuration.

The following resources are used:

* `ConfigMap` — stores the `PYTHONUNBUFFERED` environment variable.
* `Secret` — stores the Django `SECRET_KEY`.
* `Deployment` — runs the ToDo application and injects values from the ConfigMap and Secret into the container.

---

## Prerequisites

Make sure that:

* Kubernetes cluster is running.
* `kubectl` is installed and configured.
* The required Kubernetes namespace exists.
* The Docker image used by the Deployment is available.

Check the Kubernetes cluster:

```bash
kubectl cluster-info
```

Check the namespace:

```bash
kubectl get namespace mateapp
```

If the namespace does not exist, create it:

```bash
kubectl create namespace mateapp
```

---

## 1. Apply the ConfigMap

Apply the ConfigMap:

```bash
kubectl apply -f configMap.yml
```

Verify that it was created:

```bash
kubectl get configmap my-config -n mateapp
```

Display its contents:

```bash
kubectl describe configmap my-config -n mateapp
```

The ConfigMap should contain:

```text
PYTHONUNBUFFERED=1
```

---

## 2. Apply the Secret

Apply the Secret:

```bash
kubectl apply -f secret.yml
```

Verify that it was created:

```bash
kubectl get secret my-secret -n mateapp
```

For more information:

```bash
kubectl describe secret my-secret -n mateapp
```

The Secret should contain the `SECRET_KEY` key.

The actual secret value is not displayed by `kubectl describe secret`.

---

## 3. Apply the Deployment

Apply the Django ToDoApp Deployment:

```bash
kubectl apply -f deployment.yml
```

Check the Deployment:

```bash
kubectl get deployment todoapp -n mateapp
```

Check the Pods:

```bash
kubectl get pods -n mateapp
```

Wait until the Pod reaches the `Running` state and becomes ready.

---

## 4. Validate the Deployment

Check the detailed Deployment status:

```bash
kubectl describe deployment todoapp -n mateapp
```

Check the Pods belonging to the Deployment:

```bash
kubectl get pods -n mateapp -l app=todoapp
```

Check the Pod details:

```bash
kubectl describe pod <pod-name> -n mateapp
```

Replace `<pod-name>` with the actual ToDoApp Pod name.

---

## 5. Validate Environment Variables

The Deployment should inject both required environment variables into the container.

Check `PYTHONUNBUFFERED`:

```bash
kubectl exec -n mateapp <pod-name> -- printenv PYTHONUNBUFFERED
```

Expected result:

```text
1
```

Check `SECRET_KEY`:

```bash
kubectl exec -n mateapp <pod-name> -- printenv SECRET_KEY
```

The command should return the configured secret value.

Do not include the actual `SECRET_KEY` value in documentation, Git commits, screenshots, or other public materials.

---

## 6. Validate Django Application

Check the application logs:

```bash
kubectl logs -n mateapp <pod-name>
```

The application should start without configuration errors.

If the application exposes a health endpoint, it can also be checked through the configured Kubernetes Service or port forwarding.

For example:

```bash
kubectl port-forward -n mateapp deployment/todoapp 8080:8080
```

Then open:

```text
http://localhost:8080/api/health
```

---

## 7. Validate ConfigMap and Secret References

Verify that the Deployment references the ConfigMap and Secret:

```bash
kubectl describe deployment todoapp -n mateapp
```

The container environment should show references to:

```text
ConfigMap: my-config
Secret: my-secret
```

The expected configuration flow is:

```text
ConfigMap
    │
    └── PYTHONUNBUFFERED
             │
             ▼
        Deployment
             │
             ▼
       Django container


Secret
    │
    └── SECRET_KEY
             │
             ▼
        Deployment
             │
             ▼
       Django container
             │
             ▼
        settings.py
```

The Django application reads `SECRET_KEY` from the environment instead of relying on a hardcoded production secret.

---

## 8. Useful Kubernetes Commands

List all resources in the namespace:

```bash
kubectl get all -n mateapp
```

Watch Pods:

```bash
kubectl get pods -n mateapp -w
```

View Deployment events:

```bash
kubectl describe deployment todoapp -n mateapp
```

View Pod logs:

```bash
kubectl logs -n mateapp <pod-name>
```

Open a shell inside the container:

```bash
kubectl exec -it -n mateapp <pod-name> -- /bin/sh
```

---

## 9. Cleanup

Remove the Deployment:

```bash
kubectl delete -f deployment.yml
```

Remove the ConfigMap:

```bash
kubectl delete -f configMap.yml
```

Remove the Secret:

```bash
kubectl delete -f secret.yml
```

If the namespace is no longer needed:

```bash
kubectl delete namespace mateapp
```

---

## Expected Result

After applying all manifests:

1. The `my-config` ConfigMap exists in the `mateapp` namespace.
2. The `my-secret` Secret exists in the `mateapp` namespace.
3. The `todoapp` Deployment is running.
4. The ToDoApp Pod is in the `Running` state.
5. `PYTHONUNBUFFERED` is available inside the container.
6. `SECRET_KEY` is available inside the container.
7. Django obtains `SECRET_KEY` from the environment.
8. The application starts successfully without configuration errors.
