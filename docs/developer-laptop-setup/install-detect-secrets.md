# Install detect-secrets

Prevent committing passwords and other sensitive information to git repositories.

The [detect-secrets tool](https://github.com/Yelp/detect-secrets) provides out-of-the-box
support for scanning git commits for different types of credentials including keywords
(e.g. 'password' or 'secret'), private SSH keys, and base64 high entropy string.

## Installation

### Install prerequisites

`detect-secrets` is written in Python and will require Python version 3 and
`pip` (the package installer for Python) to be installed on your system.

1. Install `python` version 3 following the [instructions](https://www.python.org/downloads/) for your system
2. Install `pip` using the `get-pip.py` script following [these instructions](https://pip.pypa.io/en/stable/installing/)

### Install detect-secrets and pre-commit

`detect-secrets` harnesses the `pre-commit` tool to set-up the git pre-commit hook
that runs `detect-secrets` on the contents of the commit.

1. Install `pre-commit` by following the [instructions](https://pre-commit.com/#install) for your system
2. Install `detect-secrets` by running:

```
pip install detect-secrets
```

## Configuration

A `pre-commit` configuration file should exist in every FFC git repository that contains the necessary information to run `detect-secrets`. Your system will need to be configured to set up the git hooks for both currently cloned, and future cloned, FFC repositories.

### Currently cloned FFC git repositories

Set up the `pre-commit` git hook to run `detect-secrets` by run the following command **in every FFC repository you have cloned on your system**:

```
pre-commit install
```

### All future cloned FFC git repositories

To automatically set up the `pre-commit` git hook to run `detect-secrets` for newly cloned repositories, set up a global template:

```
git config --global init.templateDir ~/.git-template
pre-commit init-templatedir ~/.git-template
```


### Notes

`pre-commit` installs a single git hook to `.git/hooks/pre-commit`

By default git only allows a single script to be run for each hook as explained
[here](https://github.com/awslabs/git-secrets/blob/master/README.rst#options-for---install).

If a repository is using a git hooks manager such as
[husky](https://www.npmjs.com/package/husky), additional configuration will be
required in order to run git hooks created by husky and git hooks created by
git-secrets.

The solution for running multiple scripts from a single hook is out of scope of
this document. However, husky provides
[options](https://www.npmjs.com/package/husky#multiple-commands) and
[this](https://stackoverflow.com/a/26624598) Stack Overflow post discusses
another.
