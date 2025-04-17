# Install Docker Desktop

Docker Desktop is a tool to support the building and sharing of containerized applications and microservices. 

It is the fastest and easiest way to get started with Docker on your local machine and also includes Compose as part of the installation.

## Installation

If not already installed as part of the Windows Subsystem for Linux (WSL) setup, follow the below steps to install Docker Desktop.

1. Download the [Docker Desktop installer](https://www.docker.com/products/docker-desktop) for your operating system.
1. Run the installer and follow the prompts to install Docker Desktop.

### WSL

Installing Docker Desktop will automatically configure WSL to use Docker.  The Docker installation will then be available in both Windows and WSL.

1. Ensure that Docker Desktop is set to use the WSL2 backend.  This can be done by right clicking the Docker Desktop icon in the system tray and selecting `Settings`.  From there, select `General` and ensure `Use the WSL 2 based engine` is checked.
1. Ensure that Docker Desktop is available to your WSL distro.  This can be done by right clicking the Docker Desktop icon in the system tray and selecting `Settings`.  From there, select `Resources`, `WSL Integration` and ensure that the distro you are using is checked.
1. If using a device that has a proxy server, such as a Defra device, you will need to configure Docker Desktop to work around the proxy to avoid network conflicts.  This can be done by right clicking the Docker Desktop icon in the system tray and selecting `Settings`.  From there, select `Resources`, `Proxies` and ensure that `Manual proxy configuration` is checked.

### Licence

Docker Desktop requires a licence to be used within Defra.  Defra developers should request they are added to the `defradigital` Docker organisation to ensure they are fully licenced to use Docker Desktop.

Supplier developers should acquire a licence through their own organisation.

## Native installation

Docker can be installed within WSL natively without Docker Desktop by following [this guide](https://docs.docker.com/engine/install/).  No additional proxy setup is required with this method.
