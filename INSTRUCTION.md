INSTRUCTION

Apply your ConfigMap:

command: kubectl apply -f configMap.yml

Apply Secret file:

command: kubectl apply -f secret.yml

Apply Deployment:

command: kubectl apply -f deployment.yml


Check ConfigMap:

command: kubectl get configmap todoapp-config -n todoapp

Check Secret:

command: kubectl get secret todoapp-secret -n todoapp

Check Deployment:

command: kubectl get deployment todoapp -n todoapp

Check Pods:

command: kubectl get pods -n todoapp


Check whether the Deployment was successfully rolled out:

command: kubectl rollout status deployment/todoapp -n todoapp


Check PYTHONUNBUFFERED:

command: kubectl exec -n todoapp deployment/todoapp -- printenv PYTHONUNBUFFERED


Check SECRET_KEY:

command: kubectl exec -n todoapp deployment/todoapp -- sh -c 'test -n "$SECRET_KEY" && echo "SECRET_KEY is set"'