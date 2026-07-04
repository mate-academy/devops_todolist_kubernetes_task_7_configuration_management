# Deploying the ToDo App to Kubernetes

All manifests are in the `mateapp` namespace.

## Deploy everything

```bash
kubectl apply -f namespace.yml
kubectl apply -f .
```

Check status:

```bash
kubectl -n mateapp get pods,hpa,cronjob,daemonset
```

## Key choices (short version)

- **Resources** (100m/128Mi requests, 250m/256Mi limits): small app, low idle need; limit gives headroom for spikes without OOM/throttling. Tune later based on `kubectl top pods`.
- **HPA** (min 2 / max 5, CPU+memory at 70%): min 2 covers "always 2 at idle" + basic redundancy; max 5 caps burst scaling; both metrics since either could bottleneck; 70% balances reacting early vs. noise.
- **Rolling update** (`maxSurge: 1`, `maxUnavailable: 0`): never drops below 2 healthy pods during a deploy, adds one extra pod at a time to progress without doubling resource use.

## Access the app

```bash
kubectl -n mateapp get nodes -o wide
```
Open `http://<node-ip>:30080/` (NodePort from `service.yml`), or on minikube: `minikube service todoapp -n mateapp`.

Alternative, no Service needed:
```bash
kubectl -n mateapp port-forward deployment/todoapp 8080:8080
```

## DaemonSet & CronJob

Both curl the `todoapp` Service internally (`todoapp.mateapp.svc.cluster.local`).

Validate:
```bash
kubectl -n mateapp logs -l app=todoapp-curl -f          # daemonset, every ~5s
kubectl -n mateapp get cronjob todoapp-health-check      # runs every 4 min
kubectl -n mateapp get jobs
kubectl -n mateapp logs job/<job-name>                   # should show /api/health response
```
History is capped at 10 successful / 5 failed jobs.

## ConfigMap & Secret

`configMap.yml` sets `PYTHONUNBUFFERED`, `secret.yml` sets `SECRET_KEY` — both wired into `deployment.yml` as env vars.

Before applying, replace the placeholder in `secret.yml`:
```bash
python -c "import secrets; print(secrets.token_urlsafe(50))"
```

In `settings.py`, replace the hardcoded key with:
```python
import os
SECRET_KEY = os.environ["SECRET_KEY"]
```

Validate:
```bash
kubectl -n mateapp get configmap todoapp-config -o yaml
kubectl -n mateapp get secret todoapp-secret -o yaml
kubectl -n mateapp exec deploy/todoapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```