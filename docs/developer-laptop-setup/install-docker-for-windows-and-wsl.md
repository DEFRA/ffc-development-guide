# Install Docker for Windows and WSL
Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package.

FFC will primarily be developing a containerised microservice ecosystem. 

# Installation
Docker currently does not work within WSL.  To work with Docker within WSL, Docker must be installed in both Windows and WSL then WSL must connect to the Windows Docker Daemon.

## Windows
1. Download Docker Desktop from https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe
1. Installation will prompt for use of Hyper-V which is required.
1. Once installed open Settings and enable "Expose Daemon on tcp://localhost:2375 without TLS".  This will allow Docker installed on WSL to communicate with Daemon.

## WSL
1. Run the following terminal commands
```
# Update the apt package list.
sudo apt-get update -y

# Install Docker's package dependencies.
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# Download and add Docker's official public PGP key.
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify the fingerprint.
sudo apt-key fingerprint 0EBFCD88

# Add the `stable` channel's Docker upstream repository.
#
# If you want to live on the edge, you can change "stable" below to "test" or
# "nightly". I highly recommend sticking with stable!
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# Update the apt package list (for the new apt repo).
sudo apt-get update -y

# Install the latest version of Docker CE.
sudo apt-get install -y docker-ce

# Allow your user to access the Docker CLI without needing root access.
sudo usermod -aG docker $USER
```

2. Close and reopen terminal
1. Connect WSL Docker to Windows Docker  
  `echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.bashrc && source ~/.bashrc`

## Install Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

To install Run below terminal commands.
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
`sudo chmod +x /usr/local/bin/docker-compose`  
`docker-compose --version`

## Setup Shared Drives
To allow Docker to access Windows drives for creating Docker Compose volumes, the drive needs to be shared.

1. In Docker for Windows Settings menu select Shared Drives
2. Select which Windows drives you'd like to share
3. Click apply and enter your Windows password to confirm
4. If you get an error stating the firewall is blocking the configuration change then run the following Powershell command as Administrator
   `Set-NetConnectionProfile -interfacealias "vEthernet (DockerNAT)" -NetworkCategory Private`
