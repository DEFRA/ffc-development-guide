# Install VS Code
Visual Studio Code is a source code editor developed by Microsoft for Windows, Linux and macOS. It includes support for debugging, embedded Git control and GitHub, syntax highlighting, intelligent code completion, snippets, and code refactoring.

# Installation
Install OS specific version via download from https://code.visualstudio.com/download

## Install StandardJS
In line with Defra's digital standards for JavaScript, StandardJS coding standards should be used.  Visual Studio has an extension to validate syntax for StandardJS.
1. Install StandardJS extension
1. Disable VS Code JavaScript Validation by adding `"javascript.validate.enable": false` to settings.json
1. Install StandardJS locally or globally
  `npm install standard`
1. Set auto fix on save by adding `"standard.autoFixOnSave": true` to settings.json

## Configure autosave
If you prefer your code to autosave, add the following to settings.json.

```
"files.autoSave": "afterDelay"
"files.autoSaveDelay": 600
```

## MacOS Specific Config
TBC

## WSL Specific Config

### Configure terminal to use WSL
1. Download wslgit.exe from https://github.com/andy-5/wslgit
1. Update settings.json with:
  ```
  "git.path": "C:\\WINDOWS\\System32\\wslgit.exe",
  "terminal.integrated.shell.windows": "C:\\WINDOWS\\System32\\wsl.exe"
  ```
3. Add or update the Windows environment variable WSLGIT_USE_INTERACTIVE_SHELL to false or 0

### Debug configuration
The simplest way to debug code within WSL is to use the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) VS Code extension.

Alternatively, you can use the [WSL workspace folder](https://marketplace.visualstudio.com/items?itemName=lfurzewaddock.vscode-wsl-workspacefolder) and update your debug configuration to use the WSL folder.  Examples below.

#### Basic
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
#### App and test
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

#### Example settings.json
```
{
    "git.path": "C:\\WINDOWS\\System32\\wslgit.exe",
    "terminal.integrated.shell.windows": "C:\\WINDOWS\\System32\\wsl.exe",
    "javascript.validate.enable": false,
    "editor.tabSize": 2,
    "standard.autoFixOnSave": true,
    "files.insertFinalNewline": true,
    "files.autoSave": "afterDelay",
    "files.autoSaveDelay": 600,
    "explorer.confirmDelete": false,
    "explorer.confirmDragAndDrop": false,
    "workbench.colorTheme": "Monokai",
    "workbench.startupEditor": "newUntitledFile",
    "git.autofetch": true,
    "javascript.format.enable": false
}
```
