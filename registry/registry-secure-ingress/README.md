# docker-registry-secure-ingress

Install a secure ingress route for our registry on our TMC guest cluster.

## Install
To create our secure ingress

```bash
ytt -f k8s | kapp deploy -a docker-secure-ingress -n default -y -f -
```

If you want to use a different namespace:

```
ytt -f k8s -v namespace=default | kapp deploy -a docker-secure-ingress -n default -y -f -
```


## Uninstall
To remove our secure ingress from your cluster:

```bash
kapp delete -a docker-secure-ingress -n default -y
```

## Configuration
This ingress/cert-manager configuration at [k8s/registry.yaml](k8s/secure-ingress.yaml)

The values used by default are in [k8s/values.yaml](k8s/values.yaml)