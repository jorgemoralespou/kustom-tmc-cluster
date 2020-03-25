# Contour on TMC cluster
Install Contour on a TMC guest cluster.

It:
- uses **tanzu-system-ingress** as namespace by default using [ytt](https://get-ytt.io/)
- adds a **privileged-role-binding-contour** to allow envoy proxy to run privileged
- Applies [kapp](https://get-kapp.io/) [ordering-groups and order-rules](https://github.com/k14s/kapp/blob/master/docs/apply-ordering.md)
- Adds **app** and **org** labels to generated resources

## Install
To install contour on your cluster

```bash
ytt -f k8s | kapp -a contour deploy -n default -y -f -
```

If you want to use a different namespace:

```
ytt -f k8s -v namespace=projectcontour | kapp -a contour deploy -n default -y -f -
```


## Uninstall
To remove contour from your cluster:

```bash
kapp -a contour delete -n default -y
```

## Configuration
This contour configuration at [k8s/contour.yaml](k8s/contour.yaml) is generated for [Contour 1.3](https://raw.githubusercontent.com/projectcontour/contour/release-1.3/examples/render/contour.yaml)

The values used by default are in [k8s/values.yaml](k8s/values.yaml)

By default, the Load Balancer will be installed as a Network Load Balancer. If you rather prefer the Classic TCP flavour, provide this flag to ytt when processing the template:

```
-v LB_Type=tcp
```

Possible values are: **nlb** and **tcp**