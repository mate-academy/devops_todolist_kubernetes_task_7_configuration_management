# Applying and validating ConfigMap and Secret

All manifests are in the `.infrastructure` directory. Run the commands from the repository root.

## Apply the changes

Create the namespace first, because every other resource lives in it:

```
kubectl apply -f .infrastructure/namespace.yml
```

Create the ConfigMap and the Secret:

```
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
```

Create the Deployment and the Services:

```
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
```

Or apply the whole directory at once. Files are applied in alphabetical order, so run the namespace command above first.

```
kubectl apply -f .infrastructure/
```

## Validate the changes

### 1. Resources exist

```
kubectl get configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp
kubectl get deployment todoapp -n todoapp
kubectl get pods -n todoapp
```

Wait until the pods are `Running` and `READY`:

```
kubectl rollout status deployment/todoapp -n todoapp
```

### 2. ConfigMap contains `PYTHONUNBUFFERED`

```
kubectl describe configmap todoapp-config -n todoapp
```

Expected: the `Data` section contains `PYTHONUNBUFFERED: 1`.

### 3. Secret contains `SECRET_KEY`

```
kubectl get secret todoapp-secret -n todoapp -o jsonpath="{.data.SECRET_KEY}"
```

The value is base64-encoded. To decode it, on Linux/macOS/Git Bash:

```
kubectl get secret todoapp-secret -n todoapp -o jsonpath="{.data.SECRET_KEY}" | base64 -d
```

On PowerShell:

```
[Text.Encoding]::UTF8.GetString([Convert]::FromBase64String((kubectl get secret todoapp-secret -n todoapp -o jsonpath="{.data.SECRET_KEY}")))
```

### 4. The Deployment uses the ConfigMap and the Secret

```
kubectl describe deployment todoapp -n todoapp
```

Expected: the container environment contains `SECRET_KEY` (from secret `todoapp-secret`) and `PYTHONUNBUFFERED` (from configmap `todoapp-config`).

### 5. Environment variables are set inside the container

```
kubectl exec -n todoapp deploy/todoapp -- printenv PYTHONUNBUFFERED SECRET_KEY
```

Expected: `1` and the secret key value.

### 6. The application works with the key from the Secret

`settings.py` reads `SECRET_KEY` from the environment, so the pod would crash with a `KeyError` if the Secret was not applied. Check that the pod is running and the logs have no errors:

```
kubectl logs deploy/todoapp -n todoapp
```

Open the application through the NodePort service (`http://localhost:30007` on Docker Desktop, or the URL from `minikube service todoapp-nodeport -n todoapp --url`), or use port-forwarding:

```
kubectl port-forward svc/todoapp-service 8080:80 -n todoapp
```

Then open http://localhost:8080.
