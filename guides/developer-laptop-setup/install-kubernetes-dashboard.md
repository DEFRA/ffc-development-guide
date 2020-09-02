# Install Kubernetes dashboard
The Kubernetes dashboard is a web-based Kubernetes user interface.

# Installation
Install the dashboard to a Kubernetes cluster using the `kubectl apply` command specified in the **Deploying the dashboard UI** section in the below link.

https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

Example:
`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml`

### Create default user and access token
Follow the guide in the below link.
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md

### Run dashboard
1. Run terminal command `kubectl proxy`
1. Access dashboard at http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
