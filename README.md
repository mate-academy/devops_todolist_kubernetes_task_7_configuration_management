# Django ToDo list

This is a todo list web application with basic features of most web apps, i.e., accounts/login, API, and interactive UI. To do this task, you will need:

- CSS | [Skeleton](http://getskeleton.com/)
- JS | [jQuery](https://jquery.com/)

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

Create a kubernetes manifest for a pod which will contain ToDo app container:

1. Fork this repository.
1. Create a `configMap.yml` file for ConfigMap resource.
1. ConfigMap requirements:
   1. ConfigMap should have a `PYTHONUNBUFFERED` values set
   1. Deployment should use this ConfigMap and set `PYTHONUNBUFFERED` environment variable
1. Create a `secret.yml` file for Secret resource.
1. Secret requirements:
   1. Secret should have a `SECRET_KEY` value set
   1. Deployment should use this Secret and set `SECRET_KEY` environment variable
   1. Application should use this secret instead of one hardcoded in `settings.py`
1. `README.md` should have commands to apply all the changes
1. `README.md` should have instructions on how to validate the changes
1. Create PR with your changes and attach it for validation on a platform.

## Apply all manifests

```
kubectl apply -f .infrastructure/
```

## How to validate the changes

```
kubectl -n todoapp get deployments
```

```
kubectl -n todoapp get pods
```

```
kubectl -n todoapp logs <pod_name>
```
