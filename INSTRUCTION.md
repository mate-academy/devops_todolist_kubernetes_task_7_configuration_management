# Kubernetes configuration instructions

## Apply

From the repository root, apply the namespace first, then the configuration
resources, and finally the workload and service:

```powershell
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
```

Before using this outside a local development cluster, replace the example
`SECRET_KEY` in `.infrastructure/secret.yml` with a long, random Django secret
key. Do not commit a production key to source control.

## Validate

Validate all manifests locally without changing the cluster:

```powershell
kubectl apply --dry-run=client -f .infrastructure/namespace.yml
kubectl apply --dry-run=client -f .infrastructure/configMap.yml
kubectl apply --dry-run=client -f .infrastructure/secret.yml
kubectl apply --dry-run=client -f .infrastructure/deployment.yml
```

After applying, wait for the Deployment and verify its environment references:

```powershell
kubectl rollout status deployment/todoapp -n todoapp
kubectl get configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp
kubectl describe deployment todoapp -n todoapp
```

`kubectl describe deployment` should show `PYTHONUNBUFFERED` sourced from
`todoapp-config` and `SECRET_KEY` sourced from `todoapp-secret`; it does not
print the secret value.
