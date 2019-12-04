# Install AWS CLI

AWS CLI is a Python package

## Prerequisites

Python 2.6.5 or higher

## Install

* `pip install awscli` **or**
* `pip3 install awscli`

## Configure

* Follow the [QuickStart instructions](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration)

Notes

* You might also be interested in [command completion](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-completion.html) for the AWS CLI.

## Test

`aws iam get-user` should return a JSON doc with your user details
