# Install kubectl
kubectl is a command line tool for interacting with Kubernetes clusters.

## Installation
Follow the [Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

**Note** For WSL use the 'Install on Linux' instructions

## Setup shell autocompletion (optional)
```
sudo sh -c 'kubectl completion bash > /etc/bash_completion.d/kubectl'
```

Notes:
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

## Add further aliases for Kubectl (optional)
Download the following file and save to your home directory.

https://github.com/ahmetb/kubectl-aliases/blob/0533366d8e3e3b3987cc1b7b07a7e8fcfb69f93c/.kubectl_aliases

Update your `.bashrc` file with the below to enable autocomplete on all aliases in the file.

```
# Kubectl
[ -f ~/.kubectl_aliases ] && source ~/.kubectl_aliases
source <(kubectl completion bash)

for a in $(sed '/^alias /!d;s/^alias //;s/=.*$//' ~/.kubectl_aliases); do
  complete -F _complete_alias "$a"
done
```

For quick switching of Kubernetes contexts and namespaces, it may be beneficial to append the following lines to the `kubectl_aliases` file.

```
alias kns='kubectl config set-context --current --namespace'
alias kc='kubectl config use-context'
```

Full details are available in this [blog post](https://ahmet.im/blog/kubectl-aliases/)
