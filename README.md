# kustom-tmc-cluster

Additional resources to be deployed on a TMC cluster

Deploy your ingress controller:
```
# nginx-ingress
ytt -f ingress/nginx-ingress/k8s | kapp deploy -a nginx-ingress -n default -y -f -
```

Test sample application without configuring domain in AWS:
```
HOST=$(kubectl get svc/nginx-ingress-controller -n tanzu-system-ingress -o jsonpath='{ .status.loadBalancer.ingress[0].hostname }')
ytt -f sample-app/k8s -v host=${HOST} | kapp deploy -a sample-app -n default -y -f -

# Now try to access ${HOST}/hello to validate ingress is working
```

Configure your domain in AWS to route to your ELB.

Now deploy your registry in secure mode (using cert-manager to provide you a Let's encrypt certificate):
```
# cert-manager
ytt -f cert-manager/k8s | kapp deploy -a cert-manager -n default -y -f -
# registry
ytt -f registry/docker-registry/k8s | kapp deploy -a docker-registry -n default -y -f -
# registry-secure
ytt -f registry/registry/secure-ingress/k8s | kapp deploy -a docker-secure-ingress -n default -y -f -
```