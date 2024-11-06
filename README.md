# Django ToDo list

# Commands to apply all the changes:

```bash
kubectl apply -f .infrastructure/
```

# Validate:

Use command:

```bash
kubectl get pods -n todoapp
```

```bash
kubectl -n todoapp logs {pod_name}
```
