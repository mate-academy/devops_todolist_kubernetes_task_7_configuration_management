# ToDo Application Instructions

## Validating the changes

First, check that all Kubernetes resources were created successfully:

```bash
kubectl get configmaps -n todoapp
kubectl get secrets -n todoapp
kubectl get deployments -n todoapp
kubectl get pods -n todoapp
```

Check the created ConfigMap:

```bash
kubectl describe configmap app-config -n todoapp
```

Alternatively, display the ConfigMap in YAML format:

```bash
kubectl get configmap app-config -n todoapp -o yaml
```

The output should contain the `PYTHONUNBUFFERED` key.

Check that the Secret exists:

```bash
kubectl get secret app-secret -n todoapp
```

Display the Secret structure:

```bash
kubectl get secret app-secret -n todoapp -o yaml
```

The value inside the `data` section is stored in Base64 format.

To decode the `SECRET_KEY` value, run:

```bash
kubectl get secret app-secret -n todoapp \
  -o jsonpath='{.data.SECRET_KEY}' | base64 --decode
```

Check that the Deployment uses the ConfigMap and Secret:

```bash
kubectl describe deployment todoapp -n todoapp
```

The environment variables section should reference:

* `PYTHONUNBUFFERED` from ConfigMap `app-config`;
* `SECRET_KEY` from Secret `app-secret`.

Get the name of a running application Pod:

```bash
kubectl get pods -n todoapp
```

Check that the environment variables are available inside the container:

```bash
kubectl exec -n todoapp <pod-name> -- printenv PYTHONUNBUFFERED
```

```bash
kubectl exec -n todoapp <pod-name> -- printenv SECRET_KEY
```

Replace `<pod-name>` with the actual name of a running Pod.

For security reasons, the real `SECRET_KEY` value should not normally be displayed in production. To verify only that the variable exists, run:

```bash
kubectl exec -n todoapp <pod-name> -- sh -c \
  'test -n "$SECRET_KEY" && echo "SECRET_KEY is set" || echo "SECRET_KEY is not set"'
```

Check the application logs for startup errors:

```bash
kubectl logs -n todoapp <pod-name>
```

Finally, forward a local port to the application:

```bash
kubectl port-forward -n todoapp <pod-name> 8080:8080
```

Open the following address in a browser:

```text
http://localhost:8080
```

If the application starts successfully and the page opens, the ConfigMap and Secret changes are working correctly.
