## Apply Manifests

```bash
# Create the namespace
kubectl apply -f .infrastructure/namespace.yml

# Create the ConfigMap
kubectl apply -f .infrastructure/configMap.yml

# Create the Secret
kubectl apply -f .infrastructure/secret.yml

# Create the ClusterIP service
kubectl apply -f .infrastructure/clusterIp.yml

# Create the Deployment
kubectl apply -f .infrastructure/deployment.yml

```

Check that everything is running:

```bash
kubectl get all -n todoapp
```

## Confirm Environment Variables in the Pod

Open a shell in one of the application pods:

```bash
kubectl -n todoapp exec -it <todoapp-pod-name> -- sh
```

Inside the pod, print all environment variables:

```bash
printenv
```

Expected Result:
 - You should see PYTHONUNBUFFERED=1 (from ConfigMap)
 - You should see SECRET_KEY set to the base64-decoded value (from Secret)

Exit the container shell:

```bash
exit
```

## Cleanup

```bash
kubectl delete -f .infrastructure/deployment.yml
kubectl delete -f .infrastructure/clusterIp.yml
kubectl delete -f .infrastructure/configMap.yml
kubectl delete -f .infrastructure/secret.yml
kubectl delete -f .infrastructure/namespace.yml
```