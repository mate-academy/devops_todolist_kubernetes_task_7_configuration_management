### Deployment

To apply the configuration, secrets, and deploy the application, execute the following commands in order from the root of the repository:
`kubectl apply -f .infrastructure/configMap.yml`
`kubectl apply -f .infrastructure/secret.yml`
`kubectl apply -f .infrastructure/deployment.yml`

### Validation

1. To verify that the application pods are successfully created and running in the cluster, check their status:
`kubectl get pods -n todoapp -o wide`

2. **Verify Environment Variables:**
To check if the ConfigMap and Secret values are correctly injected into the container, execute a shell inside the pod and inspect the environment:
`kubectl exec -n todoapp -it <pod-name> -- env`