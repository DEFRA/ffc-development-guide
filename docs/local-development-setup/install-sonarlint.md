# Install SonarLint

[SonarLint](https://www.sonarlint.org/) is an IDE extension that identifies code quality issues as you code.

It can be configured to run in `connected mode` which allows rules configured in a SonarCloud instance to be applied to a local workspace, flagging up issues as the developer writes code.

This guide will demonstrate how to install the SonarLint extension in VS Code.  For other IDEs, refer to the [SonarLint documentation](https://www.sonarlint.org/).

## Dependencies

- Java Runtime Environment v17+

With Ubuntu, the open source version of the Java Runtime Environment (JRE) can be installed using the following command.

```
sudo apt-get install openjdk-17-jre
```

## SonarLint Installation (VS Code)

1. install [SonarLint](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode) extension

2. set location of JRE in VS Code settings.  The below example is the install location of the above command
   ```json
   "sonarlint.ls.javaHome": "/usr/lib/jvm/java-17-openjdk-amd64"
   ```
   **Note to WSL users** ensure that you update the **remote** `settings.json` file if you wish to use SonarLint in your linux environment.

This will give you Sonar code analysis using default quality gates for languages supported by SonarLint.  If you wish to use `connected mode` to sync your local workspace with a SonarCloud project, follow the below steps.

1. within the SonarCloud UI, navigate to your account's [security settings](https://sonarcloud.io/account/security/)

2. enter a token name and generate a token, noting the token value securely

3. within VS Code, add a SonarCloud server connection to `settings.json` to enable `connected mode`
   ```json
   "sonarlint.connectedMode.connections.sonarcloud": [{
    "organizationKey": "defra",
    "token": "MY_TOKEN"
   }]
   ```

4. within each project workspace, create or edit your workspace `settings.json` file to include a project link
   ```json
   "sonarlint.connectedMode.project": {
    "projectKey": "ffc-demo-payment-web"
   }
   ```
**Note** if you have multiple SonarCloud instances then a `connectionId` property can be added to the two code snippets above to correctly bind a project to the correct SonarCloud instance.

5. update project bindings by selecting `Update all project bindings to SonarCloud` from the VS Code `Command Palette`.  (`ctrl + shift + p` to open)

Rules, quality gates, exclusions etc set in the SonarCloud project will now be applied to local workspace.

## Configure Sonar for C#

The SonarLint extension for VS Code does not currently support C#, but we can still get Sonar linting rules set up in VS Code by adding the Sonar analyser to Omnisharp.

1. Add the Sonar Analyzer NuGet package if not already present in the `csproj` file:

```
dotnet add <PROJECT_NAME> package SonarAnalyzer.CSharp
```

2. Enable analysers in VS Code: Search for Omnisharp in settings and select `Enable Roslyn Analysers`. [Alternative configuration is available](https://www.strathweb.com/2019/04/roslyn-analyzers-in-code-fixes-in-omnisharp-and-vs-code/) outside of VS Code.

When VS Code analyses C# code, it will now check against the standard set of Sonar rules for C#. The Sonar rule IDs are prefixed with `S` when displayed in VS Code. For more details on the rules you can consult the [Sonar rules documentation](https://rules.sonarsource.com/csharp).
