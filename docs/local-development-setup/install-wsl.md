# Install Windows Subsystem for Linux (WSL)

Windows Subsystem for Linux (WSL) is a feature of Windows that allows you to run a Linux environment on your Windows machine, without the need for a separate virtual machine or dual booting.

WSL is designed to provide a seamless and productive experience for developers who want to use both Windows and Linux at the same time.

[Official Documentation](https://docs.microsoft.com/en-us/windows/wsl/about)

## WSL1 or WSL2

WSL2 is the default and built using a full Linux kernal and is optimised for size and performance.  It also solves many of the networking and Docker integration challenges that were present in WSL1.

WSL1 requires significantly more manual setup.  The remainder of this guide will assume WSL2 is being used.

For further information on the differences between WSL1 and WSL2, see the [Microsoft documentation](https://learn.microsoft.com/en-us/windows/wsl/compare-versions).

## Installation

Follow the below Microsoft guide to install the distro of your choice. The Ubuntu distro is recommended.

[Installation Guide](https://learn.microsoft.com/en-us/windows/wsl/install)

Within the guide there is also a [recommended setup](https://docs.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-user-info) of a development environment using WSL.

This guide should also be followed as it includes setup of Docker Desktop, Git, Windows Terminal and VS Code.

### Quick links

- [Setup Docker Desktop](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers)
- [Setup Git](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-git)
- [Setup Visual Studio Code](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)
- [Setup Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/get-started)
