# Install Kubernetes dashboard
The Kubernetes dashboard is a web-based Kubernetes user interface.

# Installation
Installation instructions are available at https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

### Create default user and access token
Follow the guide in the below link.
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md

### Run dashboard
1. Run terminal command `kubectl proxy`
1. Access dashboard at http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
