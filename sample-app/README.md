# sample application

Sample application to test a deployment/ingress

It:
- uses **sample-app** as namespace by default using [ytt](https://get-ytt.io/)
- Adds **app** and **org** labels to generated resources

## Install
To install sample-app on your cluster

```bash
ytt -f k8s | kapp deploy -a sample-app -n default -y -f -
```

If you want to use a different domain(**host**):

```
ytt -f k8s -v host=afdac51958154413dbc3e4f15f9f8360-1708978934.eu-west-1.elb.amazonaws.com | kapp deploy -a sample-app -n default -y -f -
```


## Uninstall
To remove the sample-app from your cluster:

```bash
kapp delete -a sample-app -n default -y
```

## Configuration
This sample-app configuration at [k8s/sample.yaml](k8s/nginx.yaml) is generated to test an ingress controller with an insecure route.

The values used by default are in [k8s/values.yaml](k8s/values.yaml)