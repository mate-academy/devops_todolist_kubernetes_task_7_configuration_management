# Django ToDo list

This is a to-do list web application with the basic features of most web apps, i.e., accounts/login, API, and interactive UI. To do this task, you will need:

- CSS | [Skeleton](http://getskeleton.com/)
- JS  | [jQuery](https://jquery.com/)

## Explore

Try it out by installing the requirements (the following commands work only with Python 3.8 and higher, due to Django 4):

```sh
pip install -r requirements.txt
```

Create a database schema:

```sh
python manage.py migrate
```

And then start the server (default is http://localhost:8000):

```sh
python manage.py runserver
```

You can now browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Apply changes

To apply changes enter this command

```sh
kubectl apply -f .infrastructure/secret.yml &&\
kubectl apply -f .infrastructure/confgiMap.yml &&\
kubectl apply -f .infrastructure/deployment.yml
```

## Validation

To validate changes follow this steps:

1. Get list of all pods:

    ```sh
    kubectl get pods -n todoapp
    ```

1. Enter into your app pod

    ```sh
    kubectl exec -it <your_pod_name> -n todoapp -- bash
    ```

1. check variables

    ```sh
    echo $SECRET_KEY
    ```

    and

    ```sh
    echo $PYTHONUNBUFFERED
    ```
