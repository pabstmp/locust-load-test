# ☸ Locust in Kubernetes

[Locust](https://locust.io/) is an open source load testing tool. 

You can use it :
- locally on your machine/servers 
- with Docker 🐳
- **with Kubernetes ☸**

Last Locust version tested: [`2.32.9`](https://github.com/locustio/locust/releases/tag/2.32.9)

## Deploy Locust to Kubernetes

### Create `Locust` namespace and switch to `Locust` namespace

```bash
kubectl create namespace locust
```
```bash
kubectl config set-context --current --namespace=locust
```

### Create a ConfigMap containing locust-tasks folder items

`locust-tasks` contains your script and dependencies for load testing.

Your script must have the name `locustfile.py`.

```bash
kubectl create configmap locust-configmap --from-file=locust-tasks/
```
### Deploy

```bash
kubectl apply -f k8s/locust-master-deployment.yaml
```
```bash
kubectl apply -f k8s/locust-worker-deployment.yaml
```

### (Optional) Activate HPA
The HPA is a controller that allows the number of pods in a deployment, replicaset, or statefulset to be automatically increased or decreased based on user-provided CPU usage or custom metrics.

```bash
kubectl apply -f k8s/locust-worker-hpa.yaml
```

### Get external IP to access to Locust portal 

```bash
kubectl get svc locust-master -o jsonpath="{.status.loadBalancer.ingress[0].ip}"
```

### Launch Locust portal

`http://[external_ip]:8089`

### Delete created ressources

```bash
kubectl delete namespace locust
```
## Useful links

- [Locust repository](https://github.com/locustio/locust)
- [Locust environment variables](https://docs.locust.io/en/stable/configuration.html?#environment-variables)
