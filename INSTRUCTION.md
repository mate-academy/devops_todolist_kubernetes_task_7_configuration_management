## Apply the changes

Create the namespace first, then apply the application resources:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml -f .infrastructure/secret.yml -f .infrastructure/deployment.yml -f .infrastructure/nodeport.yml
```

## Validate the changes

Confirm that all expected resources exist in the `todoapp` namespace:

```bash
kubectl get configmap todoapp-config secret todoapp-secrets deployment todoapp service todoapp-nodeport -n todoapp
```

Confirm the ConfigMap contains the required key without exposing unrelated data:

```bash
kubectl get configmap todoapp-config -n todoapp -o jsonpath='{.data.PYTHONUNBUFFERED}'
Write-Output ""
```

Confirm the Deployment references the ConfigMap and Secret:

```bash
kubectl get deployment todoapp -n todoapp -o yaml | Select-String -Pattern 'todoapp-config|todoapp-secrets|PYTHONUNBUFFERED|SECRET_KEY'
```

Wait for the rollout to complete and inspect the pod:

```bash
kubectl rollout status deployment/todoapp -n todoapp
kubectl get pods -n todoapp -l app=todoapp -o wide
kubectl describe deployment todoapp -n todoapp
```

Verify that the application pod is ready and that its health endpoints respond:

```bash
kubectl port-forward service/todoapp-nodeport 8080:80 -n todoapp
```

In a second terminal, run:

```bash
Invoke-WebRequest http://localhost:8080/api/health -UseBasicParsing
Invoke-WebRequest http://localhost:8080/api/ready -UseBasicParsing
```

Finally, verify that Django reads `SECRET_KEY` from the environment rather than a hardcoded value:

```bash
Select-String -Path src/todolist/settings.py -Pattern 'SECRET_KEY.*os.environ'
if ($LASTEXITCODE -ne 0) { throw 'settings.py does not read SECRET_KEY from the environment' }
```
