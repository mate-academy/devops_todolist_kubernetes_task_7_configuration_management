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

Create a kubernetes manifest for a pod which will contain a ToDo app container:

1. Fork this repository.
2. Create a `configMap.yml` file for ConfigMap resource.
3. ConfigMap requirements:

    3.1.  ConfigMap should have a `PYTHONUNBUFFERED` values set

    3.2.  Deployment shoyld use this ConfigMap and set `PYTHONUNBUFFERED` environment variable

4. Create a `secret.yml` file for Secret resource.
5. Secret requirements:

    5.1.  Secret should have a `SECRET_KEY` value set

    5.2.  Deployment should use this Secret and set `SECRET_KEY` environment variable

    5.3.  Application should use this secret instead of one hardcoded in `settings.py`

6. `README.md` should have commands to apply all the changes
7. `README.md` should have instructuions on how to validate the changes
8. Create PR with your changes and attach it for validation on a platform.


---
# **\\/ SOLUTION \\/**

1. We make changes to settings.py

    * The key was copied and converted

      ```
      powershell_7: 
      [convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes("e2(yx)v&tgh3_s=0yja-i!dpebxsz^dg47x)-k&kq_3zf*9e*"))

      > ZTIoeXgpdiZ0Z2gzX3M9MHlqYS1pIWRwZWJ4c3peZGc0N3gpLWsma3FfM3pmKjllKg==
      ```
    * The new env introduced
2. We apply our configurations

    `kubectl apply -f .\.infrastructure\confgiMap.yml`

    `kubectl apply -f .\.infrastructure\secret.yml` *previously converted Security Key is used
3. We delete env PYTHONBUFFERED from dockerfile since we no more need it there
4. We rebuild our dockerfile with changes in settings and dockerfile itself
5. Retag and push it to docker Hub
6. Set new image in Deployment.yml
7. Apply Deployment

    `kubectl apply -f .\.infrastructure\deployment.yml`
8. Make simple check of our todoapp 

    `kubectl apply -f .\.infrastructure\nodeport.yml`

    `http://localhost:30007/`

    `http://localhost:30007/api/health`

    `http://localhost:30007/api/ready`

    All working
9. check tht our configs are present in todoapp

    `kubectl exec -it todoapp-8c895dcd4-jms6d -n todoapp -- sh`

    ```
    # printenv

    KUBERNETES_SERVICE_PORT=443
    KUBERNETES_PORT=tcp://10.96.0.1:443
    TODOAPP_NODEPORT_SERVICE_HOST=10.99.68.136
    HOSTNAME=todoapp-8c895dcd4-jms6d
    SECRET_KEY=e2(yx)v&tgh3_s=0yja-i!dpebxsz^dg47x)-k&kq_3zf*9e*	<<<--- Over Here
    PYTHON_PIP_VERSION=23.0.1
    HOME=/root
    PYTHONUNBUFFERED=1												<<<--- Over Here
    GPG_KEY=E3FF2839C048B25C084DEBE9B26995E310250568
    TODOAPP_NODEPORT_SERVICE_PORT=80
    TODOAPP_NODEPORT_PORT=tcp://10.99.68.136:80
    TODOAPP_NODEPORT_PORT_80_TCP_ADDR=10.99.68.136
    PYTHON_GET_PIP_URL=https://github.com/pypa/get-pip/raw/dbf0c85f76fb6e1ab42aa672ffca6f0a675d9ee4/public/get-pip.py
    TERM=xterm
    KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
    TODOAPP_NODEPORT_PORT_80_TCP_PORT=80
    TODOAPP_NODEPORT_PORT_80_TCP_PROTO=tcp
    PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    KUBERNETES_PORT_443_TCP_PORT=443
    KUBERNETES_PORT_443_TCP_PROTO=tcp
    LANG=C.UTF-8
    TODOAPP_NODEPORT_PORT_80_TCP=tcp://10.99.68.136:80
    PYTHON_VERSION=3.8.19
    PYTHON_SETUPTOOLS_VERSION=57.5.0
    KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
    KUBERNETES_SERVICE_PORT_HTTPS=443
    KUBERNETES_SERVICE_HOST=10.96.0.1
    PWD=/app
    PYTHON_GET_PIP_SHA256=dfe9fd5c28dc98b5ac17979a953ea550cec37ae1b47a5116007395bfacff2ab9
    ```