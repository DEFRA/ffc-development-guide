# Install `detect-secrets`
Prevent committing passwords and other sensitive information to git repositories.

The [detect-secrets tool](https://github.com/Yelp/detect-secrets) provides out-of-the-box
support for scanning git commits for different types of credentials including keywords
(e.g. 'password' or 'secret'), private SSH keys, and base64 high entropy string.

## Installation
### Install prerequisites
`detect-secrets` is written in Python and will require Python version 3 and
`pip` (the package installer for Python) to be installed on your system.

### Python
Install `python` version 3, download the latest version for your operating system
- Windows - https://www.python.org/downloads/windows/
- Mac OS - https://www.python.org/downloads/mac-osx/

 Once downloaded, run the .exe (windows) or .pkg (macOS) file. Follow the on screen prompts, after successful installation run the following commands to confirm Python was successfully installed.
  - Windows - Open Powershell/Command Prompt and type `python --verison`, it should report the version
  - Mac Os - Open a Terminal and type `python –version`, it should report the version

#### WSL
If using WSL2 with Docker Desktop then python should already be installed.  However, if needed it can be added with the following guide:
  - WSL - https://docs.python-guide.org/starting/install3/linux/

Once successfully installed, run the command `python3 –-version`, it should report the version

### pip

Install `pip` using the `get-pip.py` script following [these instructions](https://pip.pypa.io/en/stable/installing/)

NOTE: pip is already installed if you are using Python 2 >=2.7.9 or Python 3 >=3.4 downloaded from python.org or if you are working in a Virtual Environment created by virtualenv or pyvenv. Just make sure to upgrade pip.

### Install detect-secrets and pre-commit
`detect-secrets` harnesses the `pre-commit` tool to set-up the git pre-commit hook
that runs `detect-secrets` on the contents of the commit.

1. Install `pre-commit` by following the [instructions](https://pre-commit.com/#install) for your system
2. Install `detect-secrets` by running:

```
pip install detect-secrets
```

## Configuration
A `pre-commit` configuration file should exist in every FFC git repository that contains the necessary information to run `detect-secrets`. See the [guide for creating FFC git repositories](../../standards/source-code.md).

Your system will need to be configured to set up the git hooks for both currently cloned, and future cloned, FFC repositories.

### Currently cloned FFC git repositories
Set up the `pre-commit` git hook to run `detect-secrets` by running the following command **in every FFC repository you have cloned on your system**:

```
pre-commit install
```

### All future cloned FFC git repositories
To automatically set up the `pre-commit` git hook to run `detect-secrets` for newly cloned repositories, set up a global template:

```
git config --global init.templateDir ~/.git-template
pre-commit init-templatedir ~/.git-template
```

### Using with other git hooks managers
`pre-commit` installs a single git hook to `.git/hooks/pre-commit`

By default git only allows a single script to be run for each hook.

If a repository is using a git hooks manager such as
[husky](https://www.npmjs.com/package/husky), additional configuration will be
required in order to run git hooks created by husky and git hooks created by
git-secrets.

The solution for running multiple scripts from a single hook is out of scope of
this document. However, husky provides
[options](https://www.npmjs.com/package/husky#multiple-commands) and
[this](https://stackoverflow.com/a/26624598) Stack Overflow post discusses
another.

## Usage
Refer to the [secrets management guide](../secrets-management.md) for details on dealing
with `detect-secrets` false positives and excludes.
