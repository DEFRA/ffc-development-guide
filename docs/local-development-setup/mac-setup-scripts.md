# Mac Setup Scripts
Mac users may want to consider the use of [Homebrew](brew.sh) as their go-to package manager which simplifies the installation of many packages, desktop applications, command line tools etc. by using a single command for the majority of if not all installations.

A set of scripts: ([dffc-mac-scripts](https://github.com/rtasalem/dffc-mac-scripts)) have been created and are free to use by anyone to aid setting up a Mac for the first time. Please refer to the repository for full instructions. These scripts cover mandatory installations that are listed within the FFC Development Guide including:
- Command Line Tools for Xcode
- Homebrew
- kubectl 
- Azure CLI 
- Helm
- Snyk CLI
- GitHub CLI
- Python
- Pip
- pre-commit 
- detect-secrets
- .NET SDK
- Docker
- Visual Studio Code. 

The scripts *do not* cover StandardJS or NVM (Node Version Manager). Please refer to the FFC Development Guide for instructions on installing StandardJS and NVM.

There is also an optional script which installs a range of mostly desktop applications and other packages that are not mandatorty but are nonetheless useful for day-to-day development.