# Install VS Code
Visual Studio Code is a source code editor developed by Microsoft for Windows, Linux and macOS. It includes support for debugging, embedded Git control and GitHub, syntax highlighting, intelligent code completion, snippets, and code refactoring.

# Installation
Install OS specific version via download from https://code.visualstudio.com/download

## Install StandardJS
In line with Defra's digital standards for JavaScript, StandardJS coding standards should be used. Visual Studio has an extension to validate syntax for StandardJS.
1. Install StandardJS extension
1. Disable VS Code JavaScript Validation by adding `"javascript.validate.enable": false` to settings.json
1. Install StandardJS locally or globally
  `npm install standard`
1. Set auto fix on save by adding `"standard.autoFixOnSave": true` to settings.json

## Install C# Extension
Add C# extension to enable .Net Core development.

https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp

## Configure autosave
If you prefer your code to autosave, add the following to settings.json.

```
"files.autoSave": "afterDelay"
"files.autoSaveDelay": 600
```

## Indentation
VS Code supports multiple indentation settings for each language by updating the `settings.json` file.

The below example sets a default indentation to two spaces, but uses four spaces for C# files as per Microsoft standards.

```
"editor.tabSize": 2,
"[csharp]":{
    "editor.tabSize": 4,
  },
```

**Note** JS standard will automatically correct JavaScript files to two space indentation.

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

Alternatively, you can use the [WSL workspace folder](https://marketplace.visualstudio.com/items?itemName=lfurzewaddock.vscode-wsl-workspacefolder) and update your debug configuration to use the WSL folder. Examples below.

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
    "[csharp]":{
      "editor.tabSize": 4,
    },
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

### Debugging .Net Core in a container
To debug .Net Core in a container you will need to install a remote debugger into the container. The below shows a Dockerfile layer that will install the **vsdbg** remote debugger.

```
RUN apt-get update \
  && apt-get install -y --no-install-recommends unzip \
  && curl -sSL https://aka.ms/getvsdbgsh | bash /dev/stdin -v latest -l /vsdbg
```

To attach the remote debugger to a process the below example debug configuration can be used.

```
{
      "name": ".NET Core Docker Attach",
      "type": "coreclr",
      "request": "attach",
      "processId": "${command:pickRemoteProcess}",
      "pipeTransport": {
        "pipeProgram": "docker",
        "pipeArgs": [
          "exec",
          "-i",
          "YOUR_CONTAINER_NAME_HERE"
        ],
        "debuggerPath": "/vsdbg/vsdbg",
        "pipeCwd": "${workspaceRoot}",
        "quoteArgs": false
      },
      "sourceFileMap": {
        "/YOUR_CONTAINER_DIRECTORY": "${workspaceFolder}/YOUR_LOCAL_DIRECTORY"
      }
```

When this debug session is used the `${command:pickRemoteProcess}` setting will open a dialog box prompting selection of a process running within the container. Select the process attached to the relevant .dll.

**Note** if your Windows user name has a space, the `${command:pickRemoteProcess}` command will not work and return a file not found error. To correct this run the following command in the command prompt to list all processes running in the container. Then replace `${command:pickRemoteProcess}` with the equivalent process Id.

`docker exec -i YOUR_CONTAINER_NAME sh -s < "C:\Users\USER_NAME_WITH_SPACE\.vscode\extensions\ms-vscode.csharp-1.21.3\scripts\remoteProcessPickerScript"`

Ensuring the version number is correct.
