# Elastic Kubernetes Service (EKS)
EKS is a managed Kubernetes service provided by AWS.

## Role Based Access Control (RBAC)
Role based access control (RBAC) should be set up to ensure developer AWS accounts are able to access the cluster via kubectl and helm.  

Further information can be found within the [AWS documentation](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html).

### Checking current user ARN
To check you are using your correct AWS account run the `aws iam get-user` command.

## Interact with EKS cluster
All of the subsequent processes require this first step to be completed as a prerequisite.  

Kubeconfig files are used to authorise and interact with a cluster.  These files are source controlled in a private GitLab repo with the same name as the cluster.

- connect to FFC AWS VPN using 2FA
- clone private GitLab repo containing kubeconfig file for relevant cluster
- set KUBECONFIG environment variable with  
  `export KUBECONFIG=/REPO_PATH/CONFIG_FILE_NAME`

### Access dashboard
- retrieve token to authorise dashboard access with  
  `kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-admin | awk '{print $1}') | grep 'token:' | cut -f2 -d: | tr -d '[:space:]'`  
- start dashboard with `kubectl proxy`
- navigate to dashboard at  
  `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#!/login`
- enter token to authorise dashboard access
  
### View cluster authorisation map
The cluster authorisation map shows user role mapping across a cluster.  

`kubectl describe configmap -n kube-system aws-auth`
