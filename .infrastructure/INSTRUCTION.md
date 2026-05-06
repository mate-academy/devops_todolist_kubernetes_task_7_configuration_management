1. Change **src/todolist/settings.py** on line 21 to:

    `SECRET_KEY = os.getenv("SECRET_KEY")`


2. To apply ConfigMap:
    ```bash
    kubectl apply -f .infrastructure/confgiMap.yml

3. To apply Secrets:
    ```bash
    kubectl apply -f .infrastructure/secret.yml
    
4. To apply deployment:
   ```bash
    kubectl apply -f .infrastructure/deployment.yml
   
---
To check your app go to http://localhost:30007/