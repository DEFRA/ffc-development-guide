# Configure NGINX Ingress Controller

An [Ingress controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) is an application that runs in a Kubernetes cluster and configures an HTTP load balancer according to Ingress resources.

In the case of NGINX, the Ingress controller is deployed in a pod along with
the load balancer.

## Installation

The documentation for [NGINX's chart](https://github.com/helm/charts/blob/master/stable/nginx-ingress/README.md) includes details on how to install it.

*TL;DR:*

`helm install stable/nginx-ingress --name nginx-ingress`
