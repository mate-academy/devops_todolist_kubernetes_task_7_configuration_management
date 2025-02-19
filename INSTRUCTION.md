The commands to apply ConfigMap and Secret requirements:
- kubectl apply -f configMap.yml
- kubectl apply -f secret.yml

The instructions on how to validate the changes
    Check if the ConfigMap and Secret exist in the Namespace:
        - kubectl get configmap -n mateapp
        - kubectl get secret -n mateapp

    Check if the environment variable is available inside the pod:
        - kubectl exec -it todoapp-74dfc584b7-sjns8 -n todoapp -- env | grep PYTHONUNBUFFERED
        - kubectl exec -it todoapp-74dfc584b7-sjns8 -n todoapp -- env | grep SECRET_KEY
