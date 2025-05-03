## 1. Apply changes
- apply secrets:  
`kubectl apply -f .infrastructure/secret.yml`  

- apply config:  
`kubectl apply -f .infrastructure/configMap.yml`

- apply deplloyment:  
`kubectl apply -f .infrastructure/deployment.yml`  

## 2. Validation:
- validate env app by connection to any pod:  
`kubectl exec -it POD_NAME -- sh  `  
-  and executing shell command:  
`printenv`  

- validate secret by command:  
`kubectl get secret app-secrets -o jsonpath=’{.data.*}’`