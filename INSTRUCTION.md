## How to apply all the changes

``

    kubectl apply -f configMap.yml
    kubectl apply -f app-secret.yml
    kubectl apply -f deployment.yml

``

## How to validate the changes

``

    kubectl get secret app-secrets -o jsonpath='{.data.kubectl get secret app-secrets -o jsonpath='{.data.SECRET_KEY}'}'

``