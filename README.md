# Django ToDo list

## Before testing
1. Get into the `.infrastructure` directory to apply manifests
2. Use `kubectl apply -f confgiMap.yml`
3. Use `kubectl apply -f secret.yml`
4. Use `kubectl apply -f deployment.yml`


## Validation the changes
1. Wait some time until pods will be running. Service should be available  at `localhost: 30007`
2. Use `kubectl get pods -n mateapp` to get the list with all pods. You will need values from the first column for the next step
3. Use `kubectl exec -it -n mateapp NAME -- sh`. The command should look like `kubectl exec -it -n mateapp kube2py-5b467d5fc-6w495 -- sh`
4. Use `printenv` when you get inside the pod to print all environment values to the terminal