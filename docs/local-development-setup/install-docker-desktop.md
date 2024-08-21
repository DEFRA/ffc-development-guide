# Install Docker Desktop

Docker Desktop is a tool for MacOS and Windows machines for the building and sharing of containerized applications and microservices. It is the fastest and easiest way to get started with Docker on your local machine.

## Installation

If not already installed as part of the Windows Subsystem for Linux (WSL) setup, follow the below steps to install Docker Desktop.

1. Download the [Docker Desktop installer](https://www.docker.com/products/docker-desktop) for your operating system.
1. Run the installer and follow the prompts to install Docker Desktop.

### WSL

1. Ensure that Docker Desktop is set to use the WSL2 backend.  This can be done by right clicking the Docker Desktop icon in the system tray and selecting `Settings`.  From there, select `General` and ensure `Use the WSL 2 based engine` is checked.
1. Ensure that Docker Desktop is available to your WSL distro.  This can be done by right clicking the Docker Desktop icon in the system tray and selecting `Settings`.  From there, select `Resources`, `WSL Integration` and ensure that the distro you are using is checked.
1. If using a device that has a proxy server, such as a Defra device, you will need to configure Docker Desktop to work around the proxy to avoid network conflicts.  This can be done by right clicking the Docker Desktop icon in the system tray and selecting `Settings`.  From there, select `Resources`, `Proxies` and ensure that `Manual proxy configuration` is checked.

