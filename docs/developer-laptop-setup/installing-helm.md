# Install Helm
Helm is a package manager for Kubernetes. Helm Charts are the package definitions which help you install and upgrade Kubernetes applications. Tiller is the server API which handles Helm commands.

# Installation
Run the following terminal commands to install Helm.
`curl -LO https://git.io/get_helm.sh`  
`chmod 700 get_helm.sh`   
`./get_helm.sh --version v2.14.3`
`helm`

Note: client and server (i.e. Helm and Tiller) must match for version numbers and helm commands will throw an error if this is not the case. Our scripts are still Helm V2 and hence the need to specify version when getting helm (the default version is v3).

Run the following terminal command to install Tiller into a cluster.  
`helm init --history-max 200`
