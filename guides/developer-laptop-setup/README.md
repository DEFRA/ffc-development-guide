# Setup developer laptop
Developers are free to use any Defra approved device as long as they adhere to Defra's guidance on [use of unmanaged devices](https://github.com/DEFRA/software-development-standards/blob/master/guides/unmanaged_devices.md). 

The most common environments are Windows 10 with Windows Subsystem for Linux (WSL) Ubuntu distro and macOS.

Visual Studio Code is the preferred code editor and what the below setup steps will assume.  However, developers are free to use any compatible equivalent if it aides in their productivity.

This guide will be targeted towards those setups. For Windows 10, all mention of terminal commands should be run in WSL unless specified.

## Microsoft InTune
Microsoft InTune allows an off network device access to Defra O365 resources such as Outlook and Sharepoint.

As InTune requires a new user profile to be created on the device, it is recommended that developers request and complete InTune enrollment before following this guide, to avoid duplicating setup effort.

See [Joiners, movers and leavers](../jlm.md) for enrollment steps.

## Configuration steps
### Windows 10
- [ ] [Install Windows Subsystem for Linux (WSL)](install-wsl.md)
- [ ] [Mount Windows drives in WSL](mount-windows-drives-in-wsl.md)
- [ ] [Install Docker Desktop](install-docker-desktop.md)
- [ ] [Configure Docker Desktop Kubernetes](configure-docker-desktop-kubernetes.md)
- [ ] [Install Git on WSL](install-git-on-wsl.md)

### MacOS
- [ ] [Setup MacBook](setup-macbook.md)

### General
- [ ] [Install Docker Compose](install-docker-compose.md)
- [ ] [Install detect-secrets](install-detect-secrets.md)
- [ ] [Install OpenVPN](install-openvpn.md)
- [ ] [Install Azure CLI](install-azure-cli.md)
- [ ] [Install kubectl](install-kubectl.md)
- [ ] [Install Node Version Manager (NVM)](install-node-version-manager.md)
- [ ] [Install .NET Core SDK](install-dotnet-sdk.md)
- [ ] [Install IDE](install-vs-code.md)
- [ ] [Install SonarLint](install-sonarlint.md)
- [ ] [Install Helm](installing-helm.md)
- [ ] [Install Snyk CLI](install-snyk.md)
- [ ] [Pact Broker](pact-broker.md)
