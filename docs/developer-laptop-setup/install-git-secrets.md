# Install git-secrets

Prevent committing passwords and other sensitive information to git
repositories.

## Installation

Install git-secrets by following the
[instructions](https://github.com/awslabs/git-secrets/blob/master/README.rst#installing-git-secrets)
for your OS.

## Configuration

Add AWS patterns to the global git config:
```
git secrets --register-aws -global
```

Install git-secrets into the required repositories:
```
cd /path/to/repo
git secrets --install
```

[Advanced configuration](https://github.com/awslabs/git-secrets/blob/master/README.rst#advanced-configuration)
is available.

### Notes

git-secrets installs the following git hooks:
* `pre-commit`
* `commit-msg`
* `prepare-commit-msg`

Should any of these hooks already exist prior to installing git-secrets,
confirmation will be required for them to be overridden.
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
