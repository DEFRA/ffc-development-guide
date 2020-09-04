# Configure Docker Desktop Kubernetes
Docker Desktop enables developers to deploy a single node cluster to their local machine.

## Installation
Open Docker Desktop settings, select Kubernetes then enable Kubernetes.

## WSL configuration
In order to interact with the cluster running in Windows, the default Kubeconfig file generated should be copied to WSL.

```
mkdir ~/.kube
cp /c/Users/[USERNAME]/.kube/config ~/.kube
```
