# Django ToDo list

This is a to-do list web application with the basic features of most web apps, i.e., accounts/login, API, and interactive UI. To do this task, you will need:

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

You can now browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Task

Create a Kubernetes manifest for a pod that will contain a ToDo app container:

1. Fork this repository.
1. Create a `confgiMap.yml` file for ConfigMap resource.
1. ConfigMap requirements:
    3.1. ConfigMap should have a `PYTHONUNBUFFERED` value set
    3.2. Deployment should use this ConfigMap and set the `PYTHONUNBUFFERED` environment variable
1. Create a `secret.yml` file for the Secret resource.
1. Secret requirements:
5.1. Secret should have a `SECRET_KEY` value set
5.2. Deployment should use this Secret and set the `SECRET_KEY` environment variable
5.3. Application should use this secret instead of one hardcoded in `settings.py`
1. Create the `INSTRUCTION.md` with commands to apply all the changes
1. `INSTRUCTION.md` should have instructions on how to validate the changes
1. Create PR with your changes and attach it for validation on a platform.


# Applying Changes

To apply the ConfigMap and Secret to your Kubernetes cluster, use the following commands:

`kubectl apply -f .infrastructure/configMap.yml -n todoapp`
`kubectl apply -f .infrastructure/secret.yml -n todoapp`
`kubectl apply -f .infrastructure/deployment.yml -n todoapp`
# Validating Changes

To validate the changes, you can check if the ConfigMap and Secret have been successfully applied:

`kubectl get configmap myapp-config`
`kubectl get secret myapp-secret`


To ensure your deployment is using these values, describe your deployment and look for the environment variables:

`kubectl describe deployment <your_deployment_name>`
