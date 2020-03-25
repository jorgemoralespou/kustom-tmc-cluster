# TODO

# nginx

Install [Nginx ingress controller](https://kubernetes.github.io/ingress-nginx) on a TMC guest cluster.

It:
- uses **tanzu-system-ingress** as namespace by default using [ytt](https://get-ytt.io/)
- adds a **privileged-role-binding-nginx-ingress** to allow nginx to run privileged (or add/drop capabilities)
- Applies [kapp](https://get-kapp.io/) [ordering-groups and order-rules](https://github.com/k14s/kapp/blob/master/docs/apply-ordering.md)
- Adds **app** and **org** labels to generated resources

## Install
To install nginx on your cluster

```bash
ytt -f k8s | kapp -a nginx-ingress deploy -n default -y -f -
```

If you want to use a different namespace:

```
ytt -f k8s -v namespace=default | kapp -a nginx-ingress deploy -n default -y -f -
```


## Uninstall
To remove nginx from your cluster:

```bash
kapp -a nginx-ingress delete -n default -y
```

## Configuration
This nginx-ingress configuration at [k8s/nginx.yaml](k8s/nginx.yaml) is generated for [Nginx Ingress Controller 0.30.0](https://kubernetes.github.io/ingress-nginx/)

The values used by default are in [k8s/values.yaml](k8s/values.yaml)

## arkade output
The nginx-ingress controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace default get services -o wide -w nginx-ingress-controller'

An example Ingress that makes use of the controller:

  apiVersion: extensions/v1beta1
  kind: Ingress
  metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
    name: example
    namespace: foo
  spec:
    rules:
      - host: www.example.com
        http:
          paths:
            - backend:
                serviceName: exampleService
                servicePort: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
        - hosts:
            - www.example.com
          secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
=======================================================================
= nginx-ingress has been installed.                                   =
=======================================================================

# If you're using a local environment such as "minikube" or "KinD",
# then try the inlets operator with "arkade install inlets-operator"

# If you're using a managed Kubernetes service, then you'll find
# your LoadBalancer's IP under "EXTERNAL-IP" via:

kubectl get svc nginx-ingress-controller

# Find out more at:
# https://github.com/helm/charts/tree/master/stable/nginx-ingress
