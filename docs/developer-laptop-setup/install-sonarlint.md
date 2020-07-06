# Install SonarLint
[SonarLint](https://www.sonarlint.org/) is an IDE extension that identifies code quality issues as you code.

It can be configured to run in `connected mode` which allows rules configured in a SonarCloud/SonarQube instance to be applied to a local workspace, flagging up issues as the developer codes.

This guide will demonstrate how to install the SonarLint extension in VS Code.  For other IDEs, refer to the [SonarLint documentation](https://www.sonarlint.org/).

## Dependencies
- Java Runtime Environment v11+

With Ubuntu, the open source version of the Java Runtime Environment (JRE) can be installed using the following command.

```
sudo apt-get install openjdk-11-jre
```

## SonarLint Installation (VS Code)
1. install [SonarLint](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode) extension
   
2. set location of JRE in VS Code settings.  The below example is the install location of the above command
   ```
   "sonarlint.ls.javaHome": "/usr/lib/jvm/java-11-openjdk-amd64"
   ```
   **Note to WSL users** ensure that you update the **remote** `settings.json` file if you wish to use SonarLint in your linux environment.

This will give you Sonar code analysis using default quality gates for languages supported by SonarLint.  If you wish to use `connected mode` to sync your local workspace with a SonarCloud project, follow the below steps.

1. within the SonarCloud UI, navigate to your account's [security settings](https://sonarcloud.io/account/security/)
   
2. enter a token name and generate a token, noting the token value securely
   
3. within VS Code, add a SonarCloud server connection to `settings.json` to enable `connected mode`
   ```
   "sonarlint.connectedMode.connections.sonarcloud": [{
    "organizationKey": "defra",
    "token": "MY_TOKEN"
   }]
   ```

4. within each project workspace, create or edit your workspace `settings.json` file to include a project link
   ```
   "sonarlint.connectedMode.project": {
    "projectKey": "ffc-demo-payment-web"
   }
   ```
**Note** if you have multiple SonarCloud instances then a `connectionId` property can be added to the two code snippets above to correctly bind a project to the correct SonarCloud instance.

5. update project bindings by selecting `Update all project bindings to SonarQube/SonarCloud` from the VS Code `Command Palette`.  (`ctrl + shift + p` to open)

Rules, quality gates, exclusions etc set in the SonarCloud project will now be applied to local workspace.
