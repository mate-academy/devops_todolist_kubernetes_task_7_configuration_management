Run commands from ./infrastructure workdir
### First of all, create this resources in exact same order:
```
    kubectl apply -f ./namespace.yml
    kubectl apply -f ./configMap.yml
    kubectl apply -f ./secret.yml
    kubectl apply -f ./deployment.yml
```

## Validating solution:
### After pod from deployment created and ready:
find name of new pod in todoapp namespace:
```angular2html
kubectl get pods -n todoapp
```
list all environments of pod:
```angular2html
kubectl exec -n todoapp [pod-name] -it printenv
```
and find Environment values from config and secret

Or you can try this to get environment you interest in:
```angular2html
kubectl exec -n todoapp todoapp-7fd5b998b7-z4wbl -- printenv SECRET_KEY

kubectl exec -n todoapp todoapp-7fd5b998b7-z4wbl -- printenv PYTHONUNBUFFERED
```