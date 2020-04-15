# Static code analysis
Static code analysis allows developers to write cleaner and safer code.  Defra's static code analysis tool of choice is [SonarCloud](https://sonarcloud.io/).

## SonarCloud
SonarCloud will check code for bugs, security vulnerabilities, code smells and test coverage. It can be used in two ways:
1. Automatic Analysis [via the github app](SonarCloud GitHub app) - suitable for uncompiled languages like Javascript/Node.js
2. Jenkins configured pipeline steps - suitable for compiled languages such as Java, C#, VB.NET, C/C++.  
 
More details on the reasons for adopting each of the above, along with advantages and limitations, are covered in the [Use of Sonar for code linting](https://eaflood.atlassian.net/wiki/spaces/FPS/pages/1642169151/Use+of+Sonar+for+code+linting) page on Confluence.

## Automatic Analysis - only for node.js repos
The approach adopted for automatic analysis is achieved through integration with the github app that handles communication with SonarCloud as part of an automatic check that takes place when a PR is created. This simply relies upon the project concerned being set up in SonarCloud for Automatic Analysis as the Defra organisation within github has been configured to use the SonarCloud app. 

For detailed configuration instructions, refer to the official [SonarCloud documentation on Automatic Analysis](https://sonarcloud.io/documentation/analysis/automatic-analysis).

## Jenkins configuration - for .NET core 
Since automatic analysis cannot be used for the .NET core repos they can be configured to run as part of an FFC CI/CD pipeline, SonarCloud must be integrated with Jenkins via the _SonarScanner_ plugin.
This plugin sets the name of the SonarCloud environment and the URL of the SonarCloud endpoint for use within Jenkins.

A stored credential (Jenkins secret) also needs to be added to the plugin configuration to authorise code analysis requests.  New tokens can be generated from the SonarCloud.io web interface.

SonarCloud tokens for Jenkins should be created under a service account and not an individual's account.

For detailed installation instructions, refer to the official [SonarCloud documentation](https://sonarcloud.io/documentation/analysis/scan/sonarscanner-for-jenkins/).

### Quality Gates
A Quality Gate can be configured in the Jenkins pipeline steps to determine a set of coding standards that must be adhered to in order for a pipeline to succeed.  Failure to meet the critiera of a Quality Gate will cause the build to fail.

A project can be configured to require one, many or no Quality Gates.  If no Quality Gates are selected, code quality is still analysed and reported upon, but builds are not blocked.

Jenkins can't be informed when the SonarCloud analysis is complete via a webhook in the way SonarQube could be due to the firewall restrictions in place on inbound traffic from the wider web. Therefore the Jenkins pipepline steps will have to poll periodically for the results to acertain if the steps have quality gate has been passed.

## Projects
SonarCloud has the concept of a `Project` which is used to monitor and report code analysis.  A project should have a one to one relationship with a repository.  Multiple repositories should not share a single project.

Projects are derived from github repositories so SonarCloud can analyse it.

## CI/CD integration
The [ffc-jenkins-pipeline-library](https://github.com/DEFRA/ffc-jenkins-pipeline-library) repository (version >= 0.0.6) has functions that can be used to trigger and wait for code analysis.

Details of how to use these can be found in the [README](https://github.com/DEFRA/ffc-jenkins-pipeline-library).

## SonarLint
SonarLint is an extension that identifies code quality issues as you code.

It can be configured to run in "connected mode" which allows rules configured in FFC's hosted SonarQube instance to be applied to local development.

Installation instructions are included within the [developer laptop setup documents](developer-laptop-setup/install-sonarlint.md).
