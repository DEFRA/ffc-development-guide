# Install AWS CLI

[AWS CLI](https://docs.aws.amazon.com/cli/index.html) is an open source tool
that enables you to interact with AWS services from the command line.

## Install

Install version 2 of the CLI. Instructions are available for
[major operating systems](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
including how to
[migrate from version 1 to version 2](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html#migrating).

## Configure and use

Additional guides are available for:

* [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)
* [Using the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-using.html)
* [Installing autocompletion](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-completion.html)
  for the AWS CLI

## Test

If user credentials (acquired via a request to CSC) have been configured
running `aws iam get-user` should return a JSON doc with your user details.

## Notes

Package managers such yum, apt-get, or Homebrew for macOS are often behind
several versions of the AWS CLI. To ensure that you have the latest version,
use the installation instructions linked to above.
