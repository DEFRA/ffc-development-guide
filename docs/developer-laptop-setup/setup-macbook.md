# Setup developer macbook
Written for use with bash, zsh instructions will be similar

1. Install developer tools: from a bash prompt, do `xcode-select --install` (more details here: http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)
2. Install nvm: from bash prompt, do
```
touch ~/.bash_profile
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
. ~/.bash_profile
```
(see https://github.com/nvm-sh/nvm#installation-and-update for full instructions, including zsh variant)

3. Install node 10:
```
nvm install 10.0
nvm use 10.0
```
4. Install Visual Studio Code (download from https://code.visualstudio.com and copy to /Applications), then install vscode-standardjs as per extension instructions
5. Install Docker desktop for Mac (https://docs.docker.com/docker-for-mac/install/)
6. Enable Kubernetes in Docker desktop: open preferences, select Kubernetes, check the 'enable Kubernetes' checkbox and click 'Apply'. Allow it to install the Kubernetes cluster.
7. Install Helm using [standard instructions](installing-helm.md)
8. Install Nginx Ingress Controller using [standard instructions](configure-nginx-ingress-controller.md)

