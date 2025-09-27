# INSTRUCTION.md

## Apply the Changes

To apply all the changes to your Kubernetes cluster, run the following commands:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/confgiMap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/nodeport.yml
```

## Validate the Changes

1. Open your browser and go to:

```
http://localhost:30007
```

2. You should see the ToDo application running.

## Notes

* To encode a string in base64 (for example, your secret key), use the following command:

```bash
echo -n 'Original string goes here' | base64
```

* Make sure your `deployment.yml` correctly references the ConfigMap and Secret environment variables.
