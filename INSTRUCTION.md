## How to use with Kubernetes

### 1. Prepare Kubernetes resources

Apply the ConfigMap and Secret (replace `<your-base64-secret-key>` with your actual base64-encoded secret):

```sh
kubectl apply -f .infrastructure/configMap.yml
kubectl apply -f .infrastructure/secret.yml
```

### 2. Deploy the application

Apply the deployment manifest:

```sh
kubectl apply -f .infrastructure/deployment.yml
```

### 3. Check pod status

```sh
kubectl get pods -n todoapp
```

### 4. Access the application

Forward the port to your local machine (if needed):

```sh
kubectl port-forward deployment/todoapp 8000:8080 -n todoapp
```

Then open [http://localhost:8000/](http://localhost:8000/) in your browser.

---

**Note:**  
- The Django `SECRET_KEY` is managed via Kubernetes Secret and is not stored in the repository.
- The application will not start unless the `SECRET_KEY` environment variable is set by Kubernetes.
- For local development, set `SECRET_KEY` manually in your environment before running Django.

Example for local run:

```sh
export SECRET_KEY=dev-secret-key
python manage.py