# Overview
The Ingress controller is an application that runs in a Kubernetes cluster and configures an HTTP load balancer according to Ingress resources. In the case of NGINX, the Ingress controller is deployed in a pod along with the load balancer.

# Installation
Run the following terminal command to add an NGINX ingress to a Kubernetes cluster.

`helm install stable/nginx-ingress --name nginx-ingress`
