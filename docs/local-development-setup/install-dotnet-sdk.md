# Install .NET SDK

The .NET SDK is a set of libraries and tools that allow developers to create .NET applications and libraries.

## Choose version (or use LTS)

Select the version required then either a package manager or binary for your OS from [Microsoft](https://dotnet.microsoft.com/download/dotnet)

**Note** On macOS, creation of symbolic links is missing from the installer after installation.
To create them manually run

```bash
ln -s /usr/local/share/dotnet/dotnet /usr/local/bin/
```

### Verify installation

Check your installation was successful with

```bash
dotnet --version
```

## Install .NET tools

As per [Microsoft's setup guide](https://docs.microsoft.com/en-us/ef/core/cli/dotnet) the Entity Framework tools allow the creation and application of code first migrations can be installed with the command

```bash
dotnet tool install --global dotnet-ef
```
