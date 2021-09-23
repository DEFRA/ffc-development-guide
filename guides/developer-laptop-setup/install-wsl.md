# Install Windows Subsystem for Linux (WSL)
The Windows Subsystem for Linux (WSL) lets developers run a GNU/Linux environment -- including most command-line tools, utilities, and applications -- directly on Windows, unmodified, without the overhead of a virtual machine.

[Official Documentation](https://docs.microsoft.com/en-us/windows/wsl/about)

## WSL1 or WSL2
If the Windows 10 device is capable of running it, WSL2 is preferred over WSL1.  This is because WSL2 is built using a full Linux kernal and is optimised for size and performance.  It also solves many of the networking and Docker integration challenges that were present in WSL1.

WSL1 requires significantly more manual setup.  The remainder of this guide will assume WSL2 is being used.

## Installation
Follow the below Microsoft guide to install the distro of your choice. The Ubuntu distro is recommended.

[Installation Guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

Within the guide there is also a [recommended setup](https://docs.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-user-info) of a development environment using WSL.

This guide should also be followed as it includes setup of both Docker Desktop and Git.

### Quick links
- [Setup Docker Desktop](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers)
- [Setup Git](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-git)
- [Setup Visual Studio Code](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)
- [Setup Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/get-started)

The guide also explains how to upgrade WSL1 to WSL2.
