## Branch naming convention
Branches should be named in the format `<ticket id>-<description of change>` in lower case.

For example, `1234567-add-dockerfile`.

## Pull Requests
When following feature branch development, pull requests should be used to collaborate and ensure code quality.

Pull requests should be created in line with [Defra's standards](https://github.com/DEFRA/software-development-standards/blob/master/processes/pull_requests.md) and reviewed in the spirit of [FFC's code review standards](code-review.md).

Pull requests should trigger a build and a PR deployment to support the review.

### Completing Pull Request
In order to complete a pull request, all of the following conditions must be met:
- at least one approver
- PR build must complete successfully
- dismiss stale reviews

On completion of a pull request a squash merge is performed and the feature/bug branch deleted. The commit message should be amended to give a concise description of the changes, rather than the default list of individual commits.
