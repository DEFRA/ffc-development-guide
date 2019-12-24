# Install Git on WSL
Git should be installed in WSL to allow Git commands to be run from the WSL terminal.  For example, when developing services in VS Code.

# Installation
Run the following terminal commands.

```
sudo apt-get update
sudo apt install git-all
```

## Use Windows Credential Manager in WSL
Git credentials configured in Windows can be used by WSL by running the below terminal command.  This means credential management does not need to be duplicated in WSL.
```
git config --global credential.helper "/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```
## Configure Windows Git line endings
If Git is also installed on Windows and you wish to use both, it is recommended to change your Git configuration running the below command in the Command Prompt.  This will ensure that LF line endings are not changed to CRLF when cloning a repository and CRLF files are not changed to LF.

`git config --global core.autocrlf input`
