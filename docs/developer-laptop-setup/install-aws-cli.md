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
of the team on your behalf. If you have been following the [guide](README.md)
you should have [OpenVPN](install-openvpn.md) installed and configured to
access AWS. If this isn't the case, please do so before continuing.

Access to resources within AWS is managed through
[IAM identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).
Your user should be setup so it can assume a/the role(s) required to access
resources such as EKS. Currently that role is `ffc-developer-role`.

The rest of this guide assumes a role will be assumed when communicating via
the CLI in order to have the correct permissions to interact with resources.
The simplest way to do this is to run `aws configure`. This will setup the
Access Keys (user specific values), region (`eu-west-2`) and output format
(`json`).
Next, you should set the role you want to assume. This is done by running
`aws configure set role_arn <role-arn-value>` which will add an entry for
`role_arn` into the AWS CLI config for the default profile. The config is
stored in a file named `config` which is located in `~/.aws/`.
Additional information is available in the
[AWS docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).

So far the configuration has been written to the `default` profile. There is
a feature called
[named profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)
and although not necessary, it is a good idea to setup named
profiles to help to manage the profile(s) you use to authenticate to AWS with.
Specifically, there might occasions when you need to use a role other than
`ffc-developer-role` to interact with resources. Named profiles makes this
simple.

The
[AWS docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration-multi-profiles)
explain how to create multiple profiles. If you have been following the guide
it is likely you will have a single profile named `default` with the
`ffc-developer-role` arn. Rather than leaving this as the default profile it is
advised to update the name of the default profile to something you will
recognise as associated to the `ffc-developer-role` e.g. `ffc-dev`. In order to
do this you can either follow the instructions above and specify the name of
the profile when running the configure command or edit the config file
directly.

Ultimately you should end up with a credentials file containing the access keys
e.g.
```
aws_access_key_id = xxxxxxxxxxxxxxxxxxxx
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

And a config file for the named profile e.g. `ffc-dev` and the `role_arn` set:
```
[profile ffc-dev]
output = json
region = eu-west-2
role_arn = <role-arn-value>
```

### Test

If the guide has been followed (and the credentials are effectively set for the
default profile, as would be the case in the example file above), running
`aws iam get-user` should return your _user_ details.
However, if there are no default credentials and they have been set for a
specific profile the profile will need to be specified i.e.
`aws iam get-user --profile <profile-name>`.

Another test can be run to check the profile has been setup correctly. Running
`aws sts get-caller-identity --profile <profile-name>` will show the details of
the credentials for the profile.

## Use

* [Using the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-using.html)
* [Installing autocompletion](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-completion.html)
  for the AWS CLI

## Notes

Package managers such yum, apt-get, or Homebrew for macOS are often behind
several versions of the AWS CLI. To ensure that you have the latest version,
use the installation instructions linked to above.
