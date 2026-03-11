To apply configMap.yml and secret.yml use commands:

    cd ./.infrastructure

    kubectl apply -f ./configMap.yml

    kubectl apply -f ./secret.yml

To validare changes you have to go into a pod. Use commands:

    To see pod's names:

    kubectl -f apply deployment.yml

    kubectl get pods (You will see pod's names. You can choose any pod from Deployment)

    kubectl exec -it <pod_name> -- sh

    printenv (envs and its values)
