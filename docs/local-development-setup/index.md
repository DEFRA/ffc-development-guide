# Setup local development environment

Developers are free to use any Defra approved device as long as they adhere to Defra's guidance on [use of unmanaged devices](https://github.com/DEFRA/software-development-standards/blob/master/guides/unmanaged_devices.md). 

The most common environments are Windows with [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/about) Ubuntu distro and macOS.

[Visual Studio Code](https://code.visualstudio.com/) is the preferred code editor.  However, developers are free to use any compatible equivalent if it aides in their productivity.

This guide will be targeted towards the above setups. For Windows, all mention of terminal commands should be run in WSL unless specified otherwise.

## Microsoft InTune
Microsoft InTune allows an off network device access to Defra O365 resources such as Outlook, Teams and SharePoint.

New InTune enrolments are currently banned which means if a user does not already have an InTune profile, they will not be able to access these resources from their device. There is no alternative at present.

InTune requires that the user profile on a device is an organisation account.  So either this must be setup when first configuring the device or a new user profile will need to be created on the device.  It is not possible to convert an existing user profile to an InTune profile.

## Setup steps
### Windows
1. [Install Windows Subsystem for Linux (WSL)](install-wsl.md)
1. [Mount Windows drives in WSL](mount-windows-drives-in-wsl.md)
1. [Add user to sudoers file (optional)](setup-sudoers.md)

### macOS
1. [Setup command line tools](setup-macos-command-line-tools.md)

### Common
1. [Install Docker Desktop](install-docker-desktop.md)
1. [Setup commit signing](sign-commits.md)
1. [Install IDE](install-vs-code.md)
1. [Install SonarLint](install-sonarlint.md)
1. [Install Docker Compose](install-docker-compose.md)
1. [Install detect-secrets](install-detect-secrets.md)
1. [Install Node Version Manager (NVM)](install-node-version-manager.md)
1. [Install .NET SDK](install-dotnet-sdk.md)
1. [Install kubectl](install-kubectl.md)
1. [Install Helm](installing-helm.md)
1. [Install Azure CLI](install-azure-cli.md)
1. [Install Snyk CLI](install-snyk.md)
1. [Install GitHub CLI](install-github.md)
1. [Install OpenVPN](install-openvpn.md)
