# INSTRUCTION.md

## Prerequisites

* Git
* Docker (optional, only needed if rebuilding the application image)
* Kubernetes cluster (Minikube, Kind, Docker Desktop Kubernetes, etc.)
* kubectl configured to access the cluster

All commands below assume resources live in the `todoapp` namespace.

## Apply the Kubernetes resources

Create the ConfigMap and Secret:

`kubectl apply -f configMap.yml -n todoapp`
`kubectl apply -f secret.yml -n todoapp`

##### Deploy (or update) the application:

`kubectl apply -f deployment.yml -n todoapp`

##### If the Deployment already exists and the environment variables were updated, restart it to pick up the new values:

`kubectl rollout restart deployment/todoapp -n todoapp`

## Validate the ConfigMap

##### Verify that the ConfigMap exists:

`kubectl get configmap -n todoapp`

##### View its contents:

`kubectl get configmap todoapp-config -n todoapp -o yaml`

##### Ensure it contains:

**PYTHONUNBUFFERED: "1"**

## Validate the Secret

##### Verify that the Secret exists:

`kubectl get secret -n todoapp`

##### View the Secret metadata:

`kubectl describe secret todoapp-secret -n todoapp`

##### Decode and verify the stored value:

`kubectl get secret todoapp-secret -n todoapp -o jsonpath="{.data.SECRET_KEY}" | base64 --decode`

## Validate the Deployment

##### Check that the Deployment and Pods are running:

`kubectl get deployments -n todoapp`
`kubectl get pods -n todoapp`

##### Get the current Pod name:

`kubectl get pods -n todoapp -l app=todoapp`


##### Verify the environment variables inside the Pod (replace `<pod-name>` with the name from the command above):

`kubectl exec -it <pod-name> -n todoapp -- printenv PYTHONUNBUFFERED`
`kubectl exec -it <pod-name> -n todoapp -- printenv SECRET_KEY`

##### Expected output:

* `PYTHONUNBUFFERED=1`
* `SECRET_KEY=<configured-secret-value>`

## Verify the application

##### Check the Pod logs to confirm the application starts without errors:

`kubectl logs <pod-name> -n todoapp`

**There should be no errors related to missing environment variables or `SECRET_KEY`.**

## Verify that the application uses the Kubernetes Secret

##### Confirm that:

* `SECRET_KEY` is not hardcoded in `settings.py`.
* `SECRET_KEY` is read from the environment, for example:

**SECRET_KEY = os.environ["SECRET_KEY"]

or

SECRET_KEY = os.getenv("SECRET_KEY")**

###### The application should start successfully using the value provided by the Kubernetes Secret rather than a hardcoded value in the source code.