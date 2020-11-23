# Install Git on WSL
When using WSL2, it is recommended that source code is cloned to the WSL filesystem and not Windows.  This will greatly increase performance and reduce the overhead in configuration such as debugging.

When using WSL1 with container volumes, source code must be cloned to the Windows file system.

Git should be installed in WSL to allow Git commands to be run from the WSL terminal. For example, when developing services in VS Code.

## Installation
Run the following terminal commands.

```
sudo apt-get update
sudo apt install git-all
```

### Use Windows Credential Manager in WSL
Git credentials configured in Windows can be used by WSL by running the below terminal command. This means credential management does not need to be duplicated in WSL.
```
git config --global credential.helper "/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```
### Configure Windows Git line endings
If Git is also installed on Windows and you wish to use both, it is recommended to change your Git configuration running the below command in the Command Prompt. This will ensure that LF line endings are not changed to CRLF when cloning a repository and CRLF files are not changed to LF.

`git config --global core.autocrlf input`

This should be set in the Git instance in both Windows and WSL.

## Cloning repositories
Because of the way WSL writes to Windows file shares, it can be common for cloning a repository to result in unexpected file permission changes. To avoid this when using WSL1 where it is necessary to source control in Windows, it is recommended to clone repositories to Windows file shares using PowerShell and not through WSL.
