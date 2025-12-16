# Setup local development environment

Defra Developers and directly appointed contractors are provided with a Defra device to use for development work.

Typically, Defra devices are not suitable for software development due to the restrictions placed on them by the default [ZScaler](https://www.zscaler.com/) profile. 

To work around this, developers are placed in an "exceptions" ZScaler profile which allows more flexibility to install and configure software on their devices.

Developers can also request the `MakeMeAdmin` app through ServiceNow to give them local admin rights on their device.

Supplier developers typically have their own devices which are used for development work and should be used in line with any commercial agreement.

Regardless of the device origin, all developers are expected to adhere to Defra's guidance on the [use of unmanaged devices](https://defra.github.io/software-development-standards/guides/unmanaged_devices/). 

## Operating systems

The most common operating systems is Windows with [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/about) Ubuntu distro or macOS.

[Visual Studio Code](https://code.visualstudio.com/) is the most popular code editor used within Defra.  However, developers are free to use any compatible equivalent if it aides in their productivity.

This guide will be targeted towards the above setups. For Windows, all mention of terminal commands should be run in WSL unless specified otherwise.

## Local development principles

- teams must not constrain their local development setup to a specific device or operating system to maintain developer mobility and agility
- local development should maximise the use of emulation and avoid tight coupling to cloud services where possible
- repositories should include full instructions for anyone to be able to easily run the service locally
