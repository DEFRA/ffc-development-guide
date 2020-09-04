# Configure Docker Desktop and Kubernetes
Kubernetes (commonly stylized as k8s) is an open-source container-orchestration system for automating application deployment, scaling, and management.

FFC's microservice ecosystem will be orchestrated within Kubernetes clusters.

# Installation
Open Docker Desktop settings, select Kubernetes then enable Kubernetes.

## WSL Specific config
### Copy Kubernetes configuration from Windows
```
mkdir ~/.kube
cp /c/Users/[USERNAME]/.kube/config ~/.kube
```
