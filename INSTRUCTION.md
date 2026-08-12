# Django Todo llist testing instructions
## 1. Applying all the manifests:
```bash
kubectl apply -f .infrastructure/{configMap.yml, secret.yml}
```

## 5. Access the Application
### Get the URL or IP address of the service:
```bash
kubectl get svc -n todoapp
```

### Make a request to the application to verify its availability (e.g., using curl or a browser). 
```bash
curl http://<EXTERNAL-IP>
```

### Ensure the application uses the SECRET_KEY from the Secret and not a hardcoded value.Check the application logs:
```bash
kubectl logs -n todoapp 
```

Confirm that the application does not return errors related to SECRET_KEY.