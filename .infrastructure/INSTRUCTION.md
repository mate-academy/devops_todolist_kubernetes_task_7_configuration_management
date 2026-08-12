To apply configMap.yml and secret.yml use commands:

    kubectl apply -f .infrastructure/configMap.yml

    kubectl apply -f .infrastructure/secret.yml

To validare changes you have to go into a pod. Use commands:

    To see pod's names:

        kubectl apply -f .infrastructure/deployment.yml

        kubectl get pods (You will see pod's names. You can choose any pod from Deployment)

        kubectl exec -it <pod_name> -- sh

        printenv (envs and its values)
