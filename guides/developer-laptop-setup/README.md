# Setup developer laptop

Developers are free to use any Defra approved device as long as they adhere to Defra's guidance on [use of unmanaged devices](https://github.com/DEFRA/software-development-standards/blob/master/guides/unmanaged_devices.md). 

The most common environments are Windows with [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/about) Ubuntu distro and macOS.

[Visual Studio Code](https://code.visualstudio.com/) is the preferred code editor and what the below setup steps will assume.  However, developers are free to use any compatible equivalent if it aides in their productivity.

This guide will be targeted towards those setups. For Windows, all mention of terminal commands should be run in WSL unless specified otherwise.

## Microsoft InTune
Microsoft InTune allows an off network device access to Defra O365 resources such as Outlook and Sharepoint.

As InTune requires a new user profile to be created on the device, it is recommended that developers request and complete InTune enrolment before following this guide, to avoid duplicating setup effort.

See [Joiners, movers and leavers](../jlm.md) for enrolment steps.

## Configuration steps
### Windows
- [ ] [Install Windows Subsystem for Linux (WSL)](install-wsl.md)
- [ ] [Mount Windows drives in WSL](mount-windows-drives-in-wsl.md)
- [ ] [Add user to sudoers file (optional)](./setup-sudoers.md)
### MacOS
- [ ] [Setup MacBook](setup-macbook.md)

### General
- [ ] [Setup commit signing](sign-commits.md)
- [ ] [Install IDE](install-vs-code.md)
- [ ] [Install SonarLint](install-sonarlint.md)
- [ ] [Install Docker Compose](install-docker-compose.md)
- [ ] [Install detect-secrets](install-detect-secrets.md)
- [ ] [Install Node Version Manager (NVM)](install-node-version-manager.md)
- [ ] [Install .NET SDK](install-dotnet-sdk.md)
- [ ] [Install kubectl](install-kubectl.md)
- [ ] [Install Helm](installing-helm.md)
- [ ] [Install Azure CLI](install-azure-cli.md)
- [ ] [Install Snyk CLI](install-snyk.md)
- [ ] [Install GitHub CLI](install-github.md)
- [ ] [Install OpenVPN](install-openvpn.md)
