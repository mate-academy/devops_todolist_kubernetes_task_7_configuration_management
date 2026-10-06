# Kubernetes ConfigMap and Secret Instructions

## 1. Overview

This Django ToDo application is deployed to a Kubernetes cluster in the `todoapp` namespace.

The application is exposed through:

- a ClusterIP service for internal cluster access;
- a NodePort service for access from the browser.

The NodePort service is available at:

```text
http://localhost:30007
```

This task adds Kubernetes configuration for application environment variables:

- `ConfigMap` stores the `PYTHONUNBUFFERED` configuration value;
- `Secret` stores the Django secret key;
- `Deployment` reads these values and passes them to the application container as environment variables.

The application image was rebuilt so that Django reads `SECRET_KEY` from the container environment instead of using a hardcoded value in `settings.py`.

The rebuilt image used by the Deployment is:

```text
zhekul/todoapp:3.1.0
```

The current configuration flow is:

```text
ConfigMap: todo-config-map
    PYTHONUNBUFFERED
          │
          ▼
Deployment ─────────────► PYTHONUNBUFFERED environment variable


Secret: todo-secret
    secret_key
          │
          ▼
Deployment ─────────────► SECRET_KEY environment variable
```

---

## 2. Create and Select the Namespace

Go to the `.infrastructure` directory:

```bash
cd .infrastructure
```

Create the namespace:

```bash
kubectl apply -f namespace.yml
```

Set `todoapp` as the default namespace for the current Kubernetes context:

```bash
kubectl config set-context --current --namespace=todoapp
```

After this, Kubernetes commands can be executed without specifying `-n todoapp`.

---

## 3. Apply ConfigMap and Secret

Create the ConfigMap:

```bash
kubectl apply -f configMap.yml
```

The ConfigMap contains:

```yaml
PYTHONUNBUFFERED: "1"
```

`PYTHONUNBUFFERED=1` forces Python to write output directly to stdout and stderr without buffering. This is useful in containers because application logs become available immediately.

Create the Secret:

```bash
kubectl apply -f secret.yml
```

The Secret stores the Django secret key as Base64-encoded data.

The Deployment maps the Secret key to the environment variable:

```text
SECRET_KEY
```

The Django application then reads `SECRET_KEY` from the environment instead of using a hardcoded value.

---

## 4. Deploy the Application

Apply the Deployment:

```bash
kubectl apply -f deployment.yml
```

Apply the ClusterIP service:

```bash
kubectl apply -f clusterIp.yml
```

Apply the NodePort service:

```bash
kubectl apply -f nodeport.yml
```

If the project uses the Horizontal Pod Autoscaler, apply it as well:

```bash
kubectl apply -f hpa.yml
```

Check the deployed resources:

```bash
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get configmaps
kubectl get secrets
```

The application Pods should reach the `Running` and `Ready` state.

---

## 5. Validate the Application in a Browser

The application is exposed through the NodePort service on port `30007`.

Open:

```text
http://localhost:30007
```

The application can also be tested through port forwarding.

Forward local port `8080` to port `80` of the ClusterIP service:

```bash
kubectl port-forward service/todoapp-service 8080:80
```

Then open:

```text
http://localhost:8080
```

The health endpoint can be checked at:

```text
http://localhost:8080/api/health
```

---

## 6. Validate the ConfigMap

Check that the ConfigMap exists:

```bash
kubectl get configmap todo-config-map
```

View its content:

```bash
kubectl get configmap todo-config-map -o yaml
```

The output should contain:

```yaml
PYTHONUNBUFFERED: "1"
```

To verify that the variable is available inside an application Pod, first get the Pod name:

```bash
kubectl get pods
```

Open a shell inside the Pod:

```bash
kubectl exec -it <pod-name> -- sh
```

Check the variable:

```bash
printenv PYTHONUNBUFFERED
```

Expected value:

```text
1
```

Exit the container:

```bash
exit
```

---

## 7. Validate the Secret

Check that the Secret exists:

```bash
kubectl get secret todo-secret
```

Do not print the Secret value directly during normal validation.

Open a shell inside an application Pod:

```bash
kubectl exec -it <pod-name> -- sh
```

Verify that the `SECRET_KEY` environment variable is set without displaying its value:

```bash
test -n "$SECRET_KEY" && echo "SECRET_KEY is set"
```

Expected output:

```text
SECRET_KEY is set
```

Exit the container:

```bash
exit
```

This confirms that the Kubernetes Secret is successfully injected into the Pod as the `SECRET_KEY` environment variable.

---

## 8. How the Deployment Uses ConfigMap and Secret

The Deployment reads `PYTHONUNBUFFERED` from the ConfigMap:

```yaml
- name: PYTHONUNBUFFERED
  valueFrom:
    configMapKeyRef:
      name: todo-config-map
      key: PYTHONUNBUFFERED
```

The Deployment reads the Django secret key from the Secret:

```yaml
- name: SECRET_KEY
  valueFrom:
    secretKeyRef:
      name: todo-secret
      key: SECRET_KEY
```

This keeps application configuration outside the Docker image and allows Kubernetes to provide the values when Pods are created.

---

## 9. Final Verification

Check all important resources:

```bash
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get configmaps
kubectl get secrets
```

Validate that:

- the application Pods are `Running` and `Ready`;
- `todo-config-map` exists;
- `todo-secret` exists;
- `PYTHONUNBUFFERED=1` is available inside the application Pod;
- `SECRET_KEY` is present inside the application Pod;
- the Django application uses the environment variable instead of a hardcoded secret;
- the application is available at `http://localhost:30007`;
- the health endpoint responds through port forwarding.
