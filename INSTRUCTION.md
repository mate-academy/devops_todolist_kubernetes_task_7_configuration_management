# 1) How to apply the changes:

    kubectl apply -f configMap.yml
    kubectl apply -f secret.yml
    kubectl apply -f deployment.yml

# 2) How to validate the changes:

    kubectl get secret secret-todoapp -o jsonpath='{.data.*}'

**Or you also can check your environment variable in that way:**

Go in our pod:

    kubectl exec -it <name_of_pod> -- sh

And in our pod, enter this command:

    printenv

**You get the list all your environment variables of your pod, there also will be your variables what you define in your configMap.yml**