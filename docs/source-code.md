# Source code
The default position for FFC is that code should be open source and hosted within GitHub.

If there is a need for a private repository, for example when source controlling infrastructure as code, that should be controlled within Defra's hosted GitLab instance or AzureDevOps Repos.

## Creating repositories
### Naming repositories
Repositories should follow the below naming convention.

`ffc-<workstream>-<service>`

For example, `ffc-elm-apply`.

Following this naming convention helps understand ownership of repositories and is also essential for teams to be able to use the [FFC Jenkins library](https://github.com/DEFRA/ffc-jenkins-pipeline-library) which is reliant on convention.

### Managing access
The [FFC GitHub team](https://github.com/orgs/DEFRA/teams/ffc) should be added with `Admin` access to all repositories hosted on GitHub. For GitLab and AzureDevOps repositories, the equivalent FFC should be used.

As per [Defra policy](https://github.com/DEFRA/dst-guides/tree/master/github), contractors should be added as contributors and should not be part of the FFC Team in GitHub.

### Branch policies
Master branches should be protected by a branch policy where merges requires a Pull Request approved by at least one other person.

Stale reviews should be automatically dismissed.

Repositories should only allow Squash merging.

### Secret detection

All developers are required to [install detect-secrets](developer-laptop-setup/install-detect-secrets.md)
when working on open-source FFC git repositories in github.

When setting up a new FFC git repository, it needs to be configured to work with the client-side `detect-secrets` tool:

1. Add the configuration file called `.pre-commit-config.yaml` in the repository root directory with the following contents (to exlude additional files/paths from secret detection update the `exclude:` regex):

```
- repo: git@github.com:Yelp/detect-secrets
  rev: v0.13.1
  hooks:
  - id: detect-secrets
    args: ['--baseline', '.secrets.baseline']
    exclude: ^package-lock.json|^.secrets.baseline
```

2. Create and add the baseline file:

```
detect-secrets scan > .secrets.baseline
```

### Licence
The Open Government Licence (OGL) Version 3 should be added to the repository.

```
The Open Government Licence (OGL) Version 3

Copyright (c) 2020 Defra

This source code is licensed under the Open Government Licence v3.0. To view this
licence, visit www.nationalarchives.gov.uk/doc/open-government-licence/version/3
or write to the Information Policy Team, The National Archives, Kew, Richmond,
Surrey, TW9 4DU.
```

### Readme
A `README.md` should be added to initialise the repository. As work begins on the repository, the README should adhere to [FFC documentation standards](documentation-standards.md).

## Branching strategies
FFC aims to be a true DevOps programme with capability to deliver value often and rapidly.

To support this, there are two branching strategy options, `Feature branch development` and `Trunk based development`. Feature branch development should be preferred as it has significantly lower risk and promotes more collaboration. However, there may be some scenarios where a trunk based development approach can deliver more value.

### Feature branch development
With feature branch development, all feature development should take place in a dedicated branch instead of the master branch.

This allows multiple developers to work on a feature whilst protecting the master branch.

New branches should be created from master for both feature development and bug fixes.

![Feature branch development](/docs/images/feature-branch-development.png)

#### Pros
- more control over Master
- increased opportunity for collaboration through pull requests
- broken builds can automatically be kept out of Master
- supports remote working and a experience variation
- features can be tested in isolated PR environment before approval

#### Cons
- feature branches can be long lived
- prone to larger merge conflicts

#### Notes
- well refined user stories can reduce feature branch scope
- good collaboration on how to implement a feature can reduce code review time

### Trunk based development
With trunk based development, developers commit code directly to the master branch.

This allows for a more rapid CI/CD approach, but needs to be well supported with robust working practices.

![Trunk based development](/docs/images/trunk-based-development.png)

#### Pros
- no long running feature branches
- changes can be implemented quickly
- less admin from branch creation and pull requests

#### Cons
- depends on developers running builds locally
- no way to automatically prevent broken builds into master branch
- difficult to support when there is experience variation or remote working

#### Notes
- feature flags and branching by abstraction can support long running changes
- pair programming can reduce risk through real time code review

## Branch naming convention
Branches should be named in the format `<project code>-<ticket id>-<description of change>` in lower case.

For example, `psd-452-add-dockerfile`.

## Pull Requests
When following feature branch development, pull requests should be used to collaborate and ensure code quality.

Pull requests should be created in line with [Defra's standards](https://github.com/DEFRA/software-development-standards/blob/master/processes/pull_requests.md) and reviewed in the spirit of [FFC's code review standards](/docs/code-review.md).

Pull requests should trigger a build and a PR deployment to support the view.

### Completing Pull Request
In order to complete a pull request, all of the following conditions must be met:
- at least one approver
- PR build must complete successfully

On completion of a pull request a squash merge should be performed and the feature/bug branch should be deleted. The commit message should be amended to give a concise description of the changes, rather than the default list of individual commits.
