# Install SonarLint
[SonarLint](https://www.sonarlint.org/) is an extension that identifies code quality issues as you code.

It can be configured to run in "connected mode" which allows rules configured in FFC's hosted SonarQube instance to be applied to local development.

## Installation (VS Code)
### Dependencies
- Java Runtime 11.

Can be downloaded from https://www.oracle.com/technetwork/java/javase/downloads/index.html.

### Installation steps
1. install SonarLint extension
2. add SonarQube and SonarCould server connections to VSCode's `settings.json` to enable connected mode
   ```
   {
    "sonarlint.connectedMode.connections.sonarqube": [
        {
            "connectionId": "defraSonarQube",
            "serverUrl": "https://sonarqube.aws-int.defra.cloud/",
            "token": "<key obtained from https://sonarqube.aws-int.defra.cloud/sonar/account/security/"
        }
    ],
    "sonarlint.connectedMode.connections.sonarcloud": [
        {
            "connectionId": "SonarCloud",
            "organizationKey": "defra",
            "token": "<key obtained from https://sonarcloud.io/account/security/>"
        }
    ]
}
   ```
3. add SonarQube project to `settings.json`
   ```
   {
    "sonarlint.connectedMode.project": {
        "serverId": "defraSonarQube",
        "projectKey": "the-project-key-on-sonarqube"
    }
   }
   ```
