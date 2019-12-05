# Install AWS CLI

AWS CLI is an open source tool (distrubuted as a Python package) which enables you to interact with AWS services from the command line. 

## Prerequisites

Python 2.6.5 or higher

## Install

* `pip install awscli` **or**
* `pip3 install awscli`

## Configure

* Follow the [QuickStart instructions](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration)
* Optionally, install [autocompletion](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-completion.html) for the AWS CLI.

## Test

`aws iam get-user` should return a JSON doc with your user details

## Notes

Package managers such yum, apt-get, or Homebrew for macOS are often behind several versions of the AWS CLI. To ensure that you have the latest version, use the installation instructions linked to above.
