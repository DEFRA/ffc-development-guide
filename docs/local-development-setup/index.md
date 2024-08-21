# Setup local development environment

Developers are free to use any Defra approved device as long as they adhere to Defra's guidance on [use of unmanaged devices](https://github.com/DEFRA/software-development-standards/blob/master/guides/unmanaged_devices.md). 

The most common environments are Windows with [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/about) Ubuntu distro or macOS.

[Visual Studio Code](https://code.visualstudio.com/) is the preferred code editor.  However, developers are free to use any compatible equivalent if it aides in their productivity.

This guide will be targeted towards the above setups. For Windows, all mention of terminal commands should be run in WSL unless specified otherwise.

## Defra devices

Standard issue Defra devices are not suitable for software development due to the restrictions placed on them by the [ZScaler](https://www.zscaler.com/) profile.

This means that developers typically use an unmanaged device provided by Defra for development work. These devices cannot access Defra resources such as email, Teams and SharePoint.  Developers are expected to use their standard issue Defra device for these resources.

A project is underway to provide developers with a managed device with an elevated ZScaler profile to resolve this issue.

### InTune

Microsoft InTune allows an off network device access to Defra resources from an unmanaged device.

New InTune enrolments are not permitted which means if a user does not already have an InTune profile, they will not be able to access these resources from their device.

InTune requires that the user profile on a device is an organisation account.  So either this must be setup when first configuring the device or a new user profile will need to be created on the device.  It is not possible to convert an existing user profile to an InTune profile.

## Supplier devices

Defra suppliers typically include an agreement about usage of their own devices.
