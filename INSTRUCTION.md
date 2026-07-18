## Apply changes

To apply changes enter this command

```sh
kubectl apply -f .infrastructure/secret.yml &&\
kubectl apply -f .infrastructure/configMap.yml &&\
kubectl apply -f .infrastructure/deployment.yml
```

## Validation

To validate changes follow this steps:

1. Get list of all pods:

    ```sh
    kubectl get pods -n todoapp
    ```

1. Enter into your app pod

    ```sh
    kubectl exec -it <your_pod_name> -n todoapp -- bash
    ```

1. check variables

    ```sh
    echo $SECRET_KEY
    ```

    and

    ```sh
    echo $PYTHONUNBUFFERED
    ```