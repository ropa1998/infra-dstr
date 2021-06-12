# infra-dstr

You should define where would this run. Our recommendation, use [minikube](https://minikube.sigs.k8s.io/docs/) to test
it locally.

## Dashboard

Use this to monitor the status of the services more comfortably

```bash
minikube dashboard
```

## Installation steps

### Helm Services

First you should install both memcached and etcd from the Bitnami Helm Chart

The etcd service should be called `etcd` and the memcached service `my-memcached`, so the GeoService can work properly

### ConfigMap

```bash
kubectl apply -f configmap.yaml
```

### Bitnami Chart

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### etcd

```bash
helm install etcd bitnami/etcd --set auth.rbac.enabled=false
```

### memcached

```bash
helm install my-memcached bitnami/memcached
```

## Our services

Standing from this project's `infra` dir run the next commands

### GeoService

```bash
kubectl apply -f geo-service-deployment.yaml
```

### AuthService

```bash
kubectl apply -f auth-service-deployment.yaml
```

### StockService

The stock service is the one we are packaging into a helm package. To use it go to
the [repo](https://github.com/ropa1998/stock-service) and download the latest package, which should be a `.tgz` file.

Then run the next command:

```bash
helm install stock ./<path-to-file>/<file-name>.tgz
```

### Loadbalancer

```bash
kubectl apply -f loadbalancer-service-deployment.yaml
```