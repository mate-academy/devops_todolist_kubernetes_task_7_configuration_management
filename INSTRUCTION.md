# Kubernetes Configuration Management — ConfigMap and Secret

This guide explains how to deploy and validate the ConfigMap and Secret configuration for the ToDo application.

## 1. ConfigMap

`.infrastructure/configMap.yml`:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myconfig
  namespace: todoapp
data:
  PYTHONUNBUFFERED: "1"
```

## 2. Secret

`.infrastructure/secret.yml`:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
  namespace: todoapp
type: Opaque
stringData:
  SECRET_KEY: "replace-with-a-training-secret-key"
```

`stringData` lets you store the value as plain text in the manifest; Kubernetes converts it internally.

Do not commit a real production secret to a public repository.

## 3. Django must use the Secret instead of a hardcoded value

Open:

```text
src/todolist/settings.py
```

Replace:

```python
SECRET_KEY = "hardcoded-value"
```

with:

```python
SECRET_KEY = os.environ["SECRET_KEY"]
```

The application now requires `SECRET_KEY` to come from the environment.

## 4. Use ConfigMap and Secret in Deployment

Add to the application container in `.infrastructure/deployment.yml`:

```yaml
env:
  - name: PYTHONUNBUFFERED
    valueFrom:
      configMapKeyRef:
        name: myconfig
        key: PYTHONUNBUFFERED

  - name: SECRET_KEY
    valueFrom:
      secretKeyRef:
        name: mysecret
        key: SECRET_KEY
```

The ConfigMap, Secret and Deployment must be in the same namespace.

## 5. Validate manifests

```bash
kubectl apply --dry-run=client -f .infrastructure/configMap.yml
kubectl apply --dry-run=client -f .infrastructure/secret.yml
kubectl apply --dry-run=client -f .infrastructure/deployment.yml
```

## 6. Apply changes

Apply ConfigMap and Secret first:

```bash
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
```

Then apply the Deployment:

```bash
kubectl apply -f .infrastructure/deployment.yml
```

Check rollout:

```bash
kubectl rollout status deployment/todoapp -n todoapp
```

Check Pods:

```bash
kubectl get pods -n todoapp
```

## 7. Validate ConfigMap

```bash
kubectl exec -n todoapp <POD_NAME> -- printenv PYTHONUNBUFFERED
```

Expected:

```text
1
```

## 8. Validate Secret safely

Do not print the actual secret value into logs.

Check that it exists:

```bash
kubectl exec -n todoapp <POD_NAME> -- sh -c 'test -n "$SECRET_KEY" && echo "SECRET_KEY is set"'
```

Expected:

```text
SECRET_KEY is set
```

Verify that the Deployment references the Secret:

```bash
kubectl get deployment todoapp -n todoapp -o yaml
```

Look for:

```yaml
secretKeyRef:
  name: mysecret
  key: SECRET_KEY
```

## 9. Validate settings.py

```bash
grep -n "SECRET_KEY" src/todolist/settings.py
```

Expected:

```python
SECRET_KEY = os.environ["SECRET_KEY"]
```

There should be no hardcoded secret value.

## 10. Test the application

```bash
kubectl port-forward -n todoapp deployment/todoapp 8080:8080
```

Open:

```text
http://localhost:8080
```

or:

```bash
curl http://localhost:8080/
```

Stop with:

```text
Ctrl+C
```

## 11. Troubleshooting

```bash
kubectl get configmap,secret -n todoapp
kubectl describe configmap myconfig -n todoapp
kubectl describe secret mysecret -n todoapp
kubectl describe deployment todoapp -n todoapp
kubectl logs -n todoapp deployment/todoapp
```

If a Pod does not start:

```bash
kubectl describe pod <POD_NAME> -n todoapp
```

## 12. Recommended apply order

```bash
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

Then verify:

```bash
kubectl get configmap -n todoapp
kubectl get secret -n todoapp
kubectl get deployment -n todoapp
kubectl get pods -n todoapp
```

## 13. Git / Pull Request

```bash
git status
```

Add only task files:

```bash
git add .infrastructure/configMap.yml
git add .infrastructure/secret.yml
git add .infrastructure/deployment.yml
git add src/todolist/settings.py
git add INSTRUCTION.md
```

Commit:

```bash
git commit -m "Add Kubernetes ConfigMap and Secret configuration"
```

Push:

```bash
git push
```

Suggested PR title:

```text
Add Kubernetes ConfigMap and Secret configuration
```

Suggested PR description:

```markdown
## Changes

- Added ConfigMap with `PYTHONUNBUFFERED`
- Added Secret with `SECRET_KEY`
- Updated Deployment to consume ConfigMap and Secret values
- Removed the hardcoded Django `SECRET_KEY`
- Updated Django settings to read `SECRET_KEY` from the environment
- Added deployment and validation instructions
```
