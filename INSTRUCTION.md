# Applying and validating the Kubernetes configuration

The manifests target the `todoapp` namespace. The Secret contains a sample
development key; replace it with a unique key before using this configuration
outside a disposable environment. A Kubernetes Secret manifest is not encrypted
just because it is stored as a Secret resource, so do not commit real production
credentials.

## Build the application image

The Django settings change must be included in the image used by the Deployment.
Build and push the updated image to a registry available to your cluster:

```powershell
docker build -t YOUR_REGISTRY/todoapp:configuration-management ./src
docker push YOUR_REGISTRY/todoapp:configuration-management
```

Set `spec.template.spec.containers[0].image` in
`.infrastructure/deployment.yml` to `YOUR_REGISTRY/todoapp:configuration-management`
before applying the Deployment.

## Apply the manifests

Run these commands from the repository root:

```powershell
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl rollout status deployment/todoapp -n todoapp
```

## Validate the configuration

Confirm that the ConfigMap contains `PYTHONUNBUFFERED` and the Secret contains
the `SECRET_KEY` entry (the Secret command reports key names, not their values):

```powershell
kubectl get configmap todoapp-config -n todoapp -o yaml
kubectl describe secret todoapp-secret -n todoapp
kubectl describe deployment todoapp -n todoapp
```

After the Deployment is ready, verify both environment values inside the
application container and confirm Django reads its key from the environment.
These checks do not print the secret value:

```powershell
kubectl exec deployment/todoapp -n todoapp -- python -c "import os; assert os.environ['PYTHONUNBUFFERED'] == '1'; print('PYTHONUNBUFFERED is set')"
kubectl exec deployment/todoapp -n todoapp -- python -c "import os; from todolist.settings import SECRET_KEY; assert SECRET_KEY == os.environ['SECRET_KEY']; print('Django SECRET_KEY is loaded from the environment')"
```
