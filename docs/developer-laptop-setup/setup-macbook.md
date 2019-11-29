# Setup developer macbook
Written for use with bash, zsh instructions will be similar.

## Install developer tools
```
  xcode-select --install
```

More details [here](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)

## Install nvm:
```
    touch ~/.bash_profile
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
    . ~/.bash_profile
```
For full instructions, including zsh variant, see [here](https://github.com/nvm-sh/nvm#installation-and-update )

## Install node 10:
```
    nvm install 10.0
    nvm use 10.0
```

## Install and configure Visual Studio Code

### Install VS Code
* Download from [here](https://code.visualstudio.com)
* Copy to /Applications

### Install StandardJS
In line with Defra's digital standards for JavaScript, StandardJS coding standards should be used.  Visual Studio has an extension to validate syntax for StandardJS.
* Install StandardJS extension for VS Code
* Disable VS Code JavaScript Validation by adding the following entry to settings.json
  * `"javascript.validate.enable": false`
* Install StandardJS npm package globally
  * `npm install -g standard`

### Configure autosave/autofix
If you prefer your code to auto save/fix, add the following to settings.json.

```
{
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 600,
  "standard.autoFixOnSave": true,
}
```

## Install Docker desktop for Mac

* Download from [here](https://docs.docker.com/docker-for-mac/install/)
* Enable Kubernetes in Docker desktop
  * Open preferences, select Kubernetes
  * Check the 'enable Kubernetes' checkbox and click 'Apply'.
  * Allow it to install the Kubernetes cluster.

## Install Helm
  * Use [standard instructions](installing-helm.md)

## Install Nginx Ingress Controller
  * Use [standard instructions](configure-nginx-ingress-controller.md)

## Install AWS CLI
  * Use [AWS documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html)
