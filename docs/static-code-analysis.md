# Static code analysis
Static code analysis allows developers to write cleaner and safer code.  Defra's static code analysis tool of choice is [SonarQube](https://www.sonarqube.org/).

## SonarQube
SonarQube will check code for bugs, security vulnerabilities, code smells and test coverage.

## Jenkins configuration
In order to run as part of an FFC CI/CD pipeline, SonarQube must be integrated with Jenkins via the SonarQube plugin.
This plugin sets the name of the SonarQube environment and the URL of the SonarQube endpoint for use within Jenkins.

A credential also needs to be added to the plugin configuration to authorise code analysis requests.  New tokens can be generated from the SonarQube UI.

SonarQube tokens for Jenkins should be created under a service account and not an individual's account.

The SonarScanner plugin must also be added to Jenkins and set to use a particular SonarQube environment.

For detailed installation instructions, refer to the official [SonarQube documentation](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-jenkins/).

## SonarQube configuration
In order to report analysis result back to Jenkins, a webhook must be added to the SonarQube configuration settings.

The URL must be in the format, `https://<jenkinsinstance>/sonarqube-webhook/`.  Note the final `/` is mandatory.

## Projects
SonarQube has the concept of a `Project` which is used to monitor and report code analysis.  A project should have a one to one relationship with a repository.  Multiple repositories should not share a single project.

The name of the project should match the name of the repository.

Projects can either be created first in SonarQube or if a pipeline references a project name that doesn't exist, SonarQube will automatically create it.

## Quality Gates
A Quality Gate can be configured in SonarQube to determine a set of coding standards that must be adhered to in order for a pipeline to succeed.  Failure to meet the critiera of a Quality Gate will cause the build to fail.

A project can be configured to require one, many or no Quality Gates.  If no Quality Gates are selected, code quality is still analysed and reported upon, but builds are not blocked.

## CI/CD integration
The [ffc-jenkins-pipeline-library](https://github.com/DEFRA/ffc-jenkins-pipeline-library) repository (version >= 0.0.6) has functions that can be used to trigger and wait for code analysis.

Details of how to use these can be found in the [README](https://github.com/DEFRA/ffc-jenkins-pipeline-library).

## SonarLint
SonarLint is an extension that identifies code quality issues as you code.

It can be configured to run in "connected mode" which allows rules configured in FFC's hosted SonarQube instance to be applied to local development.

Installation instructions are included within the [developer laptop setup documents](developer-laptop-setup/install-sonarlint.md).
