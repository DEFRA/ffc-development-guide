# Source code
The default position for FFC is that code should be open source and hosted within GitHub.

If there is a need for a private repository then an AzureDevOps Repo should be used instead.

## Creating repositories
### Naming repositories
Repositories follow the below naming convention.

`ffc-<workstream>-<service>`

For example, `ffc-elm-apply`.

Following this naming convention helps understand ownership of repositories and is also essential for automated secret scanning managed in Jenkins.

### Managing access
The [FFC GitHub team](https://github.com/orgs/DEFRA/teams/ffc) is added with `Admin` access to all repositories hosted on GitHub. For AzureDevOps repositories, the equivalent `FFC` team should be used.

As per [Defra policy](https://github.com/DEFRA/dst-guides/tree/master/github), contractors should be added as contributors and should not be part of the FFC Team in GitHub.

### Branch policies
- main branches are protected by a branch policy where merges requires a Pull Request approved by at least one other person.

- stale reviews are automatically dismissed.

- repositories only allow Squash merging.

- as the FFC team is added with `Write` role.

- must require signed commit

### Secret detection
All developers are required to [install detect-secrets](../guides/developer-laptop-setup/install-detect-secrets.md)
when working on open-source FFC git repositories in github.

When setting up a new FFC git repository, it needs to be configured to work with the client-side `detect-secrets` tool:

1. Add the configuration file called `.pre-commit-config.yaml` to the repository root directory with the following contents:

```
repos:
- repo: https://github.com/Yelp/detect-secrets
  rev: v0.14.3
  hooks:
  - id: detect-secrets
    args: ['--baseline', '.secrets.baseline']
```

2. Create and add the baseline file. To exclude files from the `detect-secrets` scan, run with `--exclude-files <regex>`. Examples:

```
detect-secrets scan > .secrets.baseline
```

```
detect-secrets scan --exclude-files package-lock.json > .secrets.baseline
```

### Licence
The Open Government Licence (OGL) Version 3 should be added to the repository.  A file verison is available in [this repository](../resources/LICENCE)

```
The Open Government Licence (OGL) Version 3

Copyright (c) 2020 Defra

This source code is licensed under the Open Government Licence v3.0. To view this
licence, visit www.nationalarchives.gov.uk/doc/open-government-licence/version/3
or write to the Information Policy Team, The National Archives, Kew, Richmond,
Surrey, TW9 4DU.
```

### Readme
A `README.md` should be added to initialise the repository. As work begins on the repository, the README must adhere to [FFC documentation standards](documentation-standards.md).

## Branch naming convention
Branches should be named in the format `<project code>-<ticket id>-<description of change>` in lower case.

For example, `psd-452-add-dockerfile`.

## Pull Requests
When following feature branch development, pull requests should be used to collaborate and ensure code quality.

Pull requests should be created in line with [Defra's standards](https://github.com/DEFRA/software-development-standards/blob/master/processes/pull_requests.md) and reviewed in the spirit of [FFC's code review standards](code-review.md).

Pull requests should trigger a build and a PR deployment to support the review.

### Completing Pull Request
In order to complete a pull request, all of the following conditions must be met:
- at least one approver
- PR build must complete successfully

On completion of a pull request a squash merge is performed and the feature/bug branch deleted. The commit message should be amended to give a concise description of the changes, rather than the default list of individual commits.
