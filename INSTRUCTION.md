# Instructions for deploying ConfigMap, Secret, and Deployment in Kubernetes

This guide contains information on how to deploy configuration resources (ConfigMap, Secret), update Deployment to use them, and verify that the changes are applied correctly.

---

## Applying configuration files

To create resources and update the deployment, run the following commands in the terminal:

```bash
# Create namespace (if not exists):
kubectl apply -f .infrastructure/namespace.yml

# Create a ConfigMap with Python environment variables:
kubectl apply -f .infrastructure/confgiMap.yml

# Create a Secret with the application's secret key:
kubectl apply -f .infrastructure/secret.yml

# Update Deployment to use ConfigMap and Secret:
kubectl apply -f .infrastructure/deployment.yml
```

## Validation and verification of changes

To verify that Deployment has successfully applied the settings from the ConfigMap and Secret, complete the following steps:

1. Check the status of the pods:

Make sure that the pods are running successfully and are in the `Running` status:

```bash
kubectl get pods -n todoapp
```

2. Check environment variables inside the container:

Select the name of a running pod and run the `env` command to check the presence and correctness of the `PYTHONUNBUFFERED` and `SECRET_KEY` variables:

```bash
kubectl exec -it <pod-name> -n todoapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```

Expected output:

```
PYTHONUNBUFFERED=1
SECRET_KEY=@e2(yx)v&tgh3_s=0yja-i!dpebxsz^dg47x)-k&kq_3zf*9e*
```

3. Verify that ConfigMap and Secret were created:

```bash
kubectl get configmap -n todoapp
kubectl get secret -n todoapp
kubectl describe configmap app-config -n todoapp
kubectl describe secret app-secret -n todoapp
```

4. Check application health endpoints:

```bash
kubectl port-forward deployment/todoapp 8080:8080 -n todoapp
```

In another terminal:

```bash
curl http://localhost:8080/api/health
curl http://localhost:8080/api/ready
```

Both endpoints should return a successful response.
