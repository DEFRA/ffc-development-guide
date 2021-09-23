# Install .NET Core SDK
The .NET Core SDK is a set of libraries and tools that allow developers to create .NET Core applications and libraries.

## Choose version (or use recommended)
Select the version required then either a package manager or binary for your OS.

https://dotnet.microsoft.com/download/dotnet

**Note** On MacOS, creation of symbolic links is missing from the installer after installation.
To create them manually run

`ln -s /usr/local/share/dotnet/dotnet /usr/local/bin/`

### Verify installation
Check your installation was successful with

`dotnet --version`

## Install .NET Core tools
As per [Microsoft's setup guide](https://docs.microsoft.com/en-us/ef/core/cli/dotnet) the Entity Framework tools allow the creation and application of code first migrations can be installed with the command

`dotnet tool install --global dotnet-ef`
