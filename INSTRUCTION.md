# Deploying the ToDo app to Kubernetes

## Apply

```bash
kubectl create namespace todoapp
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

## Validate

1. Check that the resources exist and the pod is `Running` / `READY 1/1`:

   ```bash
   kubectl get configmap,secret,deployment,pods -n todoapp
   kubectl rollout status deployment/todoapp -n todoapp
   ```

2. Check that the environment variables are set inside the container:

   ```bash
   kubectl exec -n todoapp deploy/todoapp -- printenv PYTHONUNBUFFERED SECRET_KEY
   ```

   Expected: `1` and the decoded secret key.

3. Check that the app responds:

   ```bash
   kubectl port-forward -n todoapp deploy/todoapp 8080:8080
   curl -i http://localhost:8080/api/
   ```

4. If the pod does not start, inspect it:

   ```bash
   kubectl describe pod -n todoapp -l app=todoapp
   kubectl logs -n todoapp -l app=todoapp
   ```

## Clean up

```bash
kubectl delete namespace todoapp
```
