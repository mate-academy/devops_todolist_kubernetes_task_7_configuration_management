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
1. `README.md` should have instructuions on how to validate the changes
1. Create PR with your changes and attach it for validation on a platform.

In order to apply changes you should execute the next steps:
kubectl delete namespace <old_namespace>
This action delete all objects that belongs to this namespace

Then apply the next yml-files
To create namespace
kubectl apply -f namespace.yml

To deploy the application
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml
kubectl apply -f hpa.yml

To get access to the application from outside (through your browser)
kubectl apply -f nodeport.yml

To check configMap
kubectl get configMap

To review environment values
kubectl get pods
kubectl exec -it <name_application_pod> -- sh

Then, inside the container, execute the command
printenv

You can see the list of environment valueses. There you can see environment values PYTHONUNBUFFERED: "1"

To review secrets values
kubectl get secrets
kubectl get secret <name_secret> -o jsonpath=’{.data.*}’

