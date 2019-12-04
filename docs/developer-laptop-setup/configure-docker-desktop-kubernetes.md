# Configure Docker Desktop and Kubernetes
Kubernetes (commonly stylized as k8s) is an open-source container-orchestration system for automating application deployment, scaling, and management.

FFC's microservice ecosystem will be orchestrated within Kubernetes clusters.

# Installation
Open Docker Desktop settings, select Kubernetes then enable Kubernetes.

## Install kubectl
Kubectl is a command line interface for running commands against Kubernetes clusters.

Run following terminal commands in WSL

### Install kubectl
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
&& chmod +x ./kubectl \
&& sudo mv ./kubectl /usr/local/bin/kubectl
```

### Setup shell autocompletion (optional)

```
sudo sh -c 'kubectl completion bash > /etc/bash_completion.d/kubectl'
```

Notes

1. assumes you are using `bash` shell
1. you will need to reload your shell for the change to be picked up

Reference

* https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion

### Add command alias for kubectl (optional)

To add the frequently used alias `k` for `kubectl` add the following lines to your `.bashrc` file (the 2nd line adds autocomplete for the alias):

```
alias k=kubectl
complete -o default -F __start_kubectl k
```

### Copy Kubernetes configuration from Windows
```
mkdir ~/.kube \
&& cp /c/Users/[USERNAME]/.kube/config ~/.kube
```

### Use Docker Desktop context in WSL
`kubectl config use-context docker-desktop`
