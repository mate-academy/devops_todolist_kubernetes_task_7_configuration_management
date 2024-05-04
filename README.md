# Django ToDo list

## Before testing
1. Get into the `.infrastructure` directory to apply manifests
2. Use `kubectl apply -f confgiMap.yml`
3. Use `kubectl apply -f secret.yml`
4. Use `kubectl apply -f deployment.yml`


## Validation the changes
1. Wait some time until pods will be running. Service should be available  at `localhost: 30007`