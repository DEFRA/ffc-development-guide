# Debugging .NET Core in a Linux container

.NET Core services can be developed using VS Code or Visual Studio.

As all FFC services are designed to be developed and run in Linux containers, 
debugging them requires the attachement of a debugger from the running container.  

In the case of .NET Core, this is dependent on a remote debugger being present in the container image.

All FFC services based on the Defra .NET Core development image include the `vsdbg` remote debugger.

## Attaching to the remote debugger

### VS Code

- add your breakpoint
- start the container with `docker-compose up --build` from the repository root directory
- in VS Code create a `launch.json` configuration similar to the below substituting the name of the container, `ffc-demo-payment-service-core`

  ```
  {
    "version": "0.2.0",
    "configurations": [
      {
        "name": ".NET Core Docker Attach",
        "type": "coreclr",
        "request": "attach",
        "processId": "${command:pickRemoteProcess}",
        "pipeTransport": {
          "pipeProgram": "docker",
          "pipeArgs": [ "exec", "-i", "ffc-demo-payment-service-core" ],
          "debuggerPath": "/vsdbg/vsdbg",
          "pipeCwd": "${workspaceRoot}",
          "quoteArgs": false
        },
        "sourceFileMap": {
          "/home/dotnet": "${workspaceFolder}"
        }
      }
    ]
  }
  ```

- start the VS Code debugger using this launch configuration
- in the context menu, select the process matching the running application, eg. `FFCDemoPaymentService`
- the breakpoint can now be hit within VS Code

### Visual Studio

Visual Studio does not integrate with the WSL filesystem, so WSL users must clone the repository in Windows to debug using Visual Studio.

For services which require environment variables to be read from the host, it is recommended to store these in a `.env` file in the repository as Docker Compose
will automatically read this file when running the container.  This file must be excluded from source control.

This process has a prerequisite of the user having [Docker Desktop](https://hub.docker.com/editions/community/docker-ce-desktop-windows/) installed which includes Docker Compose by default.

- add your break point
- using Powershell, start the container with `docker-compose up --build` from the repository root directory
- in Visual Studio, select `Debug -> Attach to process`
- select `Docker (Linux Container)` for `Connection type`
- type the name of the container in `Connection target`, eg. `ffc-demo-payment-service`
- click `Refresh`
- select process matching running application, eg `FFCDemoPaymentService`
- click `Attach`
- select `Managed (.NET Core for Unix)` code type
- click `Ok`
- the breakpoint can now be hit within Visual Studio

**Note** volume mounts do not appear to work with this approach, so for changes to be picked up, the container will need to be recreated.
