1) Apply manifests to create the `todoapp` namespace and deploy the resources:
```bash
kubectl apply -f ./.infrastructure/namespace.yml
kubectl apply -f ./.infrastructure/
```
2) Validate variables by the "printenv" command inside the container:
```bash
kubectl get pods -n todoapp
kubectl exec -it <pod_name> -- sh
printenv
```
