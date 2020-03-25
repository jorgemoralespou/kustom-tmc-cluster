# docker-registry

Install [docker-registry](https://docs.docker.com/registry) on a TMC guest cluster.

It:
- uses **tanzu-system-docker-registry** as namespace by default using [ytt](https://get-ytt.io/)
- Adds **app** and **org** labels to generated resources

## Install
To install docker-registry on your cluster

```bash
ytt -f k8s | kapp deploy -a docker-registry -n default -y -f -
```

If you want to use a different namespace:

```
ytt -f k8s -v namespace=default | kapp deploy -a docker-registry -n default -y -f -
```


## Uninstall
To remove docker-registry from your cluster:

```bash
kapp delete -a docker-registry -n default -y
```

## Configuration
This docker-registry configuration at [k8s/registry.yaml](k8s/registry.yaml) is generated for [2.7.1](https://docs.docker.com/registry)

The values used by default are in [k8s/values.yaml](k8s/values.yaml)

By default the registry is configured with the following credentials in the htpasswd file:

* User: **admin**
* Password: **admin**

###  To generate the htpasswd file

```
docker run --entrypoint htpasswd registry:2 -Bbn <username> <password>
```