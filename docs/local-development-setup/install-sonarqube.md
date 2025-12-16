# Install SonarQube for IDE

SonarQube Cloud is a cloud-based code quality and security analysis platform that helps developers identify and fix issues in their code. It provides a comprehensive set of tools for analyzing code quality, including static code analysis, code coverage, and security vulnerability detection.

All Defra projects are required to setup their repositories within the SonarQube Cloud [Defra organisation](https://sonarcloud.io/organizations/defra/projects)

[SonarQube for IDE](https://www.sonarsource.com/products/sonarlint/) is an IDE extension that identifies code quality issues as you code.

## Dependencies

- Java Runtime Environment v17+

With Ubuntu, the open source version of the Java Runtime Environment (JRE) can be installed using the following command.

```bash
sudo apt-get install openjdk-17-jre
```

## VS Code

1. install [SonarQube IDE](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode) extension

2. set location of JRE in VS Code settings.  The below example is the install location of the above command
   ```json
   {
     "sonarlint.ls.javaHome": "/usr/lib/jvm/java-11-openjdk-amd64"
   }
   ```

This will give you Sonar code analysis using default quality gates for languages supported by SonarQube.

### Connected mode

Connected mode binds the extension to the actual project in SonarQube Cloud.  This allows SonarQube IDE to use the same rules, quality gates, and exclusions as the SonarQube project.

Follow the [documentation](https://docs.sonarsource.com/sonarqube-for-ide/vs-code/team-features/connected-mode-setup/#connection-setup) to connect your SonarQube IDE to the SonarQube Cloud project.
