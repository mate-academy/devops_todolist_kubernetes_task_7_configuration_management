1. Commands to apply all the changes.

    - Apply ConfigMap:

        cd .\.infrastructure\ && kubectl apply -f configMap.yml

    - Apply Secrets:

        cd .\.infrastructure\ && kubectl apply -f secret.yml

    - Apply changes in the deployment.yml:

        cd .\.infrastructure\ && kubectl apply -f deployment.yml


2. Instructions on how to validate the changes.

    - Checking for the presence of env 'PYTHONUNBUFFERED' in the configMap.yml:

        kubectl get configmap config-map -o jsonpath='{.data.PYTHONUNBUFFERED}' -n todoapp

    - Checking for the presence of env 'PYTHONUNBUFFERED' in the Pod:

        kubectl get pods -n todoapp && kubectl -n todoapp exec -it <pod> -- sh && printenv | grep PYTHONUNBUFFERED
    
    - Checking for the presence of env 'SECRET_KEY' in the secret.yml:

        kubectl get secret config-secret -o jsonpath='{.data.*}' -n todoapp

    - Checking for the presence of env 'SECRET_KEY' in the Pod:

        kubectl get pods -n todoapp && kubectl -n todoapp exec -it <pod> -- sh && printenv | grep SECRET_KEY