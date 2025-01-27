### The instructions on how to deploy daemonset.yml and cronjob.yml to the cluster.
```bash
   kubectl apply -k .\.infrastructure\
```
### Instructions on how to validate the changes
```bash
    kubectl exec -it -n todoapp todoapp-58b4c86644-dk9jr  -- printenv
```
### The instructions on how to validate the solution daemonset and cronjob.
    kubectl logs <name_pod_daemonset> --tail=100 | findstr curl
    kubectl logs <name_cronejob_pod> --tail=5 | findstr Health
