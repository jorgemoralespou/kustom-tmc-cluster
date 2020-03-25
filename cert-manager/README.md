# cert-manager

Install [cert-manager 0.12.0](https://cert-manager.io/) on a TMC guest cluster.

It:
- uses **tanzu-system-cert-manager** as namespace by default using [ytt](https://get-ytt.io/)
- Applies [kapp](https://get-kapp.io/) [ordering-groups and order-rules](https://github.com/k14s/kapp/blob/master/docs/apply-ordering.md)
- Adds **app** and **org** labels to generated resources

## Install
To install nginx on your cluster

```bash
ytt -f k8s | kapp deploy -a cert-manager -n default -y -f -
```

If you want to use a different namespace:

```
ytt -f k8s -v namespace=default | kapp deploy -a cert-manager -n default -y -f -
```


## Uninstall
To remove nginx from your cluster:

```bash
kapp delete -a cert-manager -n default -y
```

## Configuration
This cert-manager configuration at [k8s/cert-manager.yaml](k8s/cert-manager.yaml) is generated for [cert-manager 0.12.0](https://cert-manager.io/)

The values used by default are in [k8s/values.yaml](k8s/values.yaml)