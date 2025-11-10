щоб перевірити confgiMap потрібно застосувати змінни 
 kubectl appy -f confgiMap.yml
 kubectl appy -f deployment.yml
anagement % kubectl get configmap    
і побачимо табличку  
NAME               DATA   AGE
*                   *      *

потім перевіряємо поди 
kubectl get pogs
-------
щоб перевірити secret  отрібно застосувати змінни 
kubectl apply -f secret.yml
kubectl apply -f deployment.yml

