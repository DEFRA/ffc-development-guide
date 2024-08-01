# Security

Teams should take a proactive approach to security, ensuring that security is considered at every stage of the development lifecycle.

## FCP Platform

The FCP Platform offers a set of secure environments consuming services will automatically inherit.

## CI Pipeline

The CI pipeline is configured to run security checks on every commit with npm audit and Snyk.  These checks will fail the build if any vulnerabilities are found beyond a `medium` severity.

## SonarCloud

SonarCloud has several security checks that will feedback to open Pull Requests on a repository.

Teams should regularly review SonarCloud for new issues detected in SonarCloud.

## Detect Secrets

All developers are required to [install Detect Secrets](../local-development-setup/install-detect-secrets.md) and configure each repository to use it to reduce the risk of secrets being committed in error.

## GitHub

GitHub will automatically scan for secrets in repositories if enabled.  Teams should ensure that this is enabled for all repositories and proactively check for new issues.

## Docker parent images

FCP must should all consume the Defra supported Docker parent images for Node.js and .NET.  These images are scanned nightly for vulnerabilities and updated regularly.

## Dealing with vulnerability alerts

If one of the above tools identifies a vulnerability, the team should take immediate action to investigate the issue to protect the service and the platform.

First, the team should determine the severity of the vulnerability, the potential impact on their service and whether it is possible to exploit.  Not all vulnerabilities are exploitable for every service.  However, teams should consider if the vulnerability could affect other services running on the Platform.

If the vulnerability is exploitable, the team should take immediate action to remediate the issue.  This may involve upgrading a package, changing a configuration or applying a patch.

Investigation may involve reading the CVE, checking the package documentation, reading issues in the relevant GitHub repository or consulting with other teams.

npm has good [guidance](https://docs.npmjs.com/auditing-package-dependencies-for-security-vulnerabilities) on how to deal with vulnerabilities.

Snyk have good [learning resources](https://snyk.io/learn/) to help teams understand vulnerabilities and how to remediate them.

For any uncertainty, teams should engage with Security for advice.

### Ignoring non-exploitable vulnerabilities

### Snyk

Vulnerabilities can be ignored through a `.snyk` file in the repository root.

> Note: for .NET, the `.snyk` file should be added in the project subfolder in a `.bin` folder as this is where Snyk will look for it for .NET.

### npm Audit

npm Audit does not support exclusions.  Currently the only way to work around a vulnerability of medium or above is to [temporarily amend](https://github.com/DEFRA/ffc-jenkins-pipeline-library/blob/master/vars/buildNodeJs.md) the `Jenkinsfile` to not fail on npm audit.

### SonarCloud

SonarCloud issues that are not exploitable can be marked as `False Positive` with a justification.
