ІНСТРУКЦІЯ З РОЗГОРТАННЯ ТА ПЕРЕВІРКИ (Configuration Management)
======================================================================

1. ПІДГОТОВКА ТА ЗАПУСК
-----------------------
Перед початком переконайтеся, що ви знаходитесь у кореневій папці проекту.
Перейдіть у директорію з конфігураційними файлами:

  cd .infrastructure

  kubectl apply -f configMap.yml
  kubectl apply -f secret.yml
  kubectl apply -f deployment.yml

2. ПЕРЕВІРКА ЗМІН
--------------------------
  kubectl get pods -n todoapp -o wide
  kubectl get secret todo-secret-key -n todoapp -o jsonpath='{.data.*}'

3. ЗАПУСК ДОДАТКУ
--------------------------
  http://localhost:30007/


