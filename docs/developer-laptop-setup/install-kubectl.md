# Install kubectl

## Prerequisites

* [AWS IAM Authenticator](install-aws-aim-authenticator.md)

## Installation

* https://kubernetes.io/docs/tasks/tools/install-kubectl/

Note: For WSL use the 'Install on Linux' instructions

## Setup shell autocompletion (optional)

```
sudo sh -c 'kubectl completion bash > /etc/bash_completion.d/kubectl'
```

Notes

1. assumes you are using `bash` shell
1. you will need to reload your shell for the change to be picked up

Reference

* https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion

## Add command alias for kubectl (optional)

To add the frequently used alias `k` for `kubectl` add the following lines to your `.bashrc` file (the 2nd line adds autocomplete for the alias):

```
alias k=kubectl
complete -o default -F __start_kubectl k
```

## WSL Specific config

### Copy Kubernetes configuration from Windows
```
mkdir ~/.kube
cp /c/Users/[USERNAME]/.kube/config ~/.kube
```

## EKS Configuration

Cluster specfic EKS `kubectl` configuration files can be downloaded [from](https://gitlab-dev.aws-int.defra.cloud/ffc/kubernetes) this private GitLab repo.

## Notes

* When connecting to an AWS cluster the kubectl config makes use of the AWS command line tool, so you need to be authenticated as your Defra AWS user as well as connected to the VPN. This is because the cluster config uses the get-token feature of the AWS command.
