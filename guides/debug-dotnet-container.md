# Debugging .NET Core in a container
To debug .NET Core in a container you will need to install a remote debugger into the container. The below shows a Dockerfile layer that will install the **vsdbg** remote debugger.

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
