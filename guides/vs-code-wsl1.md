# Using Visual Studio Code with WSL1
With WSL1, as source code will be in Windows, depending on the development activity it may be easier to use WSL in Windows rather than using a remote WSL session.

## Configure terminal to use WSL
1. Download wslgit.exe from https://github.com/andy-5/wslgit
1. Update settings.json with:
  ```
  "git.path": "C:\\WINDOWS\\System32\\wslgit.exe",
  "terminal.integrated.shell.windows": "C:\\WINDOWS\\System32\\wsl.exe"
  ```
3. Add or update the Windows environment variable WSLGIT_USE_INTERACTIVE_SHELL to false or 0

## Debug configuration
With WSL1, you can use the [WSL workspace folder](https://marketplace.visualstudio.com/items?itemName=lfurzewaddock.vscode-wsl-workspacefolder) and update your debug configuration to use the WSL folder. Examples below.

### Basic
```
{
  "type": "node",
  "request": "launch",
  "name": "WSL - Launch Program",
  "useWSL": true,
  "localRoot": "${workspaceFolder}",
  "remoteRoot": "${command:extension.vscode-wsl-workspaceFolder}",
  "program": "${workspaceFolder}\\index.js"
}
```
### App and test
```
{
  "type": "node",
  "request": "launch",
  "name": "WSL - Launch Program",
  "useWSL": true,
  "localRoot": "${workspaceFolder}",
  "remoteRoot": "${command:extension.vscode-wsl-workspaceFolder}",
  "program": "${workspaceFolder}\\index.js",
  "env" : {
    "PASSWORD": "secret",
    "PORT": "3000",
    "WSLENV": "PASSWORD:PORT"
  }
},
{
  "type": "node",
  "request": "launch",
  "name": "WSL - Lab Tests",
  "useWSL": true,
  "localRoot": "${workspaceFolder}",
  "remoteRoot": "${command:extension.vscode-wsl-workspaceFolder}",
  "program": "${workspaceFolder}/node_modules/lab/bin/lab",
  "args": [
  "-v",
],
"internalConsoleOptions": "openOnSessionStart"
}
```
- The env section is used to pass environment variables to the debugger
- `"internalConsoleOptions": "openOnSessionStart"` allows F5 to start debugger.
