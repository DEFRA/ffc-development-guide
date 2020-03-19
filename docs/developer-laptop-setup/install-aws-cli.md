# Install AWS CLI

[AWS CLI](https://docs.aws.amazon.com/cli/index.html) is an open source tool
that enables you to interact with AWS services from the command line.

## Install

Install version 2 of the CLI. Instructions are available for
[major operating systems](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
including how to
[migrate from version 1 to version 2](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html#migrating).

## Configure

In order to configure the CLI, AWS credentials are required. These are acquired
through a request to Cloud Service Center (CSC) either by yourself or a member
of the team on your behalf.

Your user will have a role assigned e.g. `ffc-developer-role`. The role
determines the access you have. When authenticating to AWS the profile
you use will need to have the correct role associated.
Additional information is available in the
[user guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html),
part of the larger set of documentation for
[configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).

The simplest way to configure your profile with a role is to run
`aws configure set role_arn <role_arn_value>`. This will add a `role_arn` entry
into the `config` file used by AWS CLI (located in `~/.aws/`).

Although not necessary, it is recommended to setup named profiles as this helps
to manage the profile(s) you use to authenticate to AWS with.
For example, it might be useful to have a profile with a role of `developer`
and another profile setup with an `operations` role. Additional information is
available in
[named profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html).

### Test

During configuration and before any named profiles are setup running
`aws iam get-user` should return your user details.
Once a profile has been setup the name of the profile will need to be specified
i.e. `aws iam get-user --profile <profile-name>`

Another way to test the setup has been successful is to run
`aws sts get-caller-identity`. Optionally specifying the profile to use e.g.
`aws sts get-caller-identity --profile <profile-name>`. This will return the
details of the user or role's credentials used to make requests.

## Use

* [Using the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-using.html)
* [Installing autocompletion](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-completion.html)
  for the AWS CLI

## Notes

Package managers such yum, apt-get, or Homebrew for macOS are often behind
several versions of the AWS CLI. To ensure that you have the latest version,
use the installation instructions linked to above.
