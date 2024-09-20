# Django ToDo list

This is a todo list web application with basic features of most web apps, i.e., accounts/login, API, and interactive UI. To do this task, you will need:

- CSS | [Skeleton](http://getskeleton.com/)
- JS  | [jQuery](https://jquery.com/)

## Explore

Try it out by installing the requirements (the following commands work only with Python 3.8 and higher, due to Django 4):

```
pip install -r requirements.txt
```

Create a database schema:

```
python manage.py migrate
```

And then start the server (default is http://localhost:8000):

```
python manage.py runserver
```

Now you can browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Task

Create a kubernetes manifest for a pod which will containa ToDo app container:

1. Fork this repository.
1. Create a `confgiMap.yml` file for ConfigMap resource.
1. ConfigMap requirements:
    3.1. ConfigMap should have a `PYTHONUNBUFFERED` values set
    3.2. Deployment shoyld use this ConfigMap and set `PYTHONUNBUFFERED` environment variable
1. Create a `secret.yml` file for Secret resource.
1. Secret requirements:
5.1. Secret should have a `SECRET_KEY` value set
5.2. Deployment should use this Secret and set `SECRET_KEY` environment variable
5.3. Application should use this secret instead of one hardcoded in `settings.py`
1. `README.md` should have commands to apply all the changes

Instruction:

Apply configMap:

```
kubectl apply -f .infrastructure/confgiMap.yml
```

Apply secret:

```
kubectl apply -f .infrastructure/secret.yml
```

Reconfig deployment:

```
kubectl apply -f .infrastructure/deployment.yml 
```

1. `README.md` should have instructuions on how to validate the changes

Check that configMap and Secret exist:

```
kubectl get configmap
```

```
kubectl get secrets
```

Check pods:

```
kubectl get pod
```

Check env variables inside a pod:

```
kubectl exec -it todoapp-{random symbols} -- sh  
```

```
printenv
```

1. Create PR with your changes and attach it for validation on a platform.
