# Overview
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

### Copy Kubernetes configuration from Windows

```
mkdir ~/.kube \
&& cp /c/Users/[USERNAME]/.kube/config ~/.kube
```

### Use Docker Desktop context in WSL

`kubectl config use-context docker-desktop`
