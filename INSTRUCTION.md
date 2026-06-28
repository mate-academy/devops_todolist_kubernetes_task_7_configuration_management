1. Commands to apply all the changes.
Need run commands:
cd .infrastructure
kubectl apply -f configMap.yml
kubectl apply -f secret.yml
kubectl apply -f deployment.yml

2. How to validate the changes.
Need run commands:
kubectl get pods -n todoapp
kubectl exec -it <pod_name> -- sh
printenv
And if you see environments: PYTHONUNBUFFERED, SECRET_KEY witch we input early, you done well.
Something like that:
C:\Mate Academy\devops_todolist_kubernetes_task_7_configuration_management\.infrastructure> kubectl exec -it todoapp-547b9dcc67-5xcdg -n todoapp -- sh
>> ,
# printenv
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.96.0.1:443
HOSTNAME=todoapp-547b9dcc67-5xcdg
SECRET_KEY=@e2(yx)v&tgh3_s=0yja-i!dpebxsz^dg47x)-k&kq_3zf*9e*
TODOAPP_SERVICE_PORT_80_TCP_ADDR=10.96.48.201
PYTHON_PIP_VERSION=23.0.1
HOME=/root
TODOAPP_SERVICE_PORT_80_TCP_PORT=80
PYTHONUNBUFFERED=1
TODOAPP_SERVICE_PORT_80_TCP_PROTO=tcp
GPG_KEY=E3FF2839C048B25C084DEBE9B26995E310250568
TODOAPP_SERVICE_PORT_80_TCP=tcp://10.96.48.201:80
PYTHON_GET_PIP_URL=https://github.com/pypa/get-pip/raw/dbf0c85f76fb6e1ab42aa672ffca6f0a675d9ee4/public/get-pip.py
TERM=xterm
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
LANG=C.UTF-8
PYTHON_VERSION=3.8.19
PYTHON_SETUPTOOLS_VERSION=57.5.0
TODOAPP_SERVICE_SERVICE_HOST=10.96.48.201
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_SERVICE_HOST=10.96.0.1
PWD=/app
PYTHON_GET_PIP_SHA256=dfe9fd5c28dc98b5ac17979a953ea550cec37ae1b47a5116007395bfacff2ab9
TODOAPP_SERVICE_SERVICE_PORT=80
TODOAPP_SERVICE_PORT=tcp://10.96.48.201:80