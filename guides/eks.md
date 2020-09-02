# Elastic Kubernetes Service (EKS)

[EKS](https://aws.amazon.com/eks/) is a managed Kubernetes service provided by
AWS.

## Role Based Access Control (RBAC)

RBAC should be set up to ensure developer AWS accounts are able to access the
cluster via kubectl and helm.

Further information can be found within the
[AWS documentation](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html).

### Checking current user ARN

To check you are using your correct AWS account run `aws iam get-user` (if
multiple profiles have been setup ensure the one with the correct role is
used).

## Interact with EKS cluster

There are at least a couple of ways to configure kubectl to have access to
the EKS cluster, two are described below. Both approaches use kubeconfig
files to authorise and interact with a cluster.

Note: access must be configured prior to subsequent steps being completed.

### Clone kubeconfig repo

There is a (private GitLab) repo, named after the cluster, containing a
kubeconfig file. Following these steps will setup kubectl to use that config:

- connect to FFC AWS VPN using 2FA
- clone private GitLab repo containing kubeconfig file for relevant cluster
- set KUBECONFIG environment variable -
  `export KUBECONFIG=/REPO_PATH/CONFIG_FILE_NAME`

### Update kubeconfig via EKS

Alternatively aws eks can be used to update kubeconfig. This is achieved by
running `aws eks --region <region-code> update-kubeconfig --name <cluster-name>`
as per
[these instructions](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html).

#### Access dashboard

- retrieve token to authorise dashboard access with
  `kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-admin | awk '{print $1}') | grep 'token:' | cut -f2 -d: | tr -d '[:space:]'`
- start dashboard with `kubectl proxy`
- navigate to dashboard at
  `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#!/login`
- enter token to authorise dashboard access

### View cluster authorisation map

The cluster authorisation map shows user role mapping across a cluster.

`kubectl describe configmap -n kube-system aws-auth`
