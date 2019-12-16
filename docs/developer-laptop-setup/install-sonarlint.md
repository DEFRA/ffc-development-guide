# Install SonarLint
[SonarLint](https://www.sonarlint.org/) is an extension that identifies code quality issues as you code.

It can be configured to run in "connected mode" which allows rules configured in FFC's hosted SonarQube instance to be applied to local development.

## Installation (VS Code)
### Dependencies
- Java Runtime 8 or 11.  

Can be downloaded from https://www.oracle.com/technetwork/java/javase/downloads/index.html.

### Installation steps
1. install SonarLint extension
2. add SonarQube server connection to `settings.json` to enable connected mode
   ```
   {
    "sonarlint.connectedMode.servers": [
        { "serverId": "mySonarQube", "serverUrl": "https://sonarqube.mycompany.com", "token": "<generated from SonarQube account/security page>" },
        { "serverId": "sonarcloud", "serverUrl": "https://sonarcloud.io", "organizationKey": "myOrgOnSonarCloud", "token": "<generated from https://sonarcloud.io/account/security/>" }
    ]
   }
   ```
3. add SonarQube project to `settings.json`
   ```
   {
    "sonarlint.connectedMode.project": {
        "serverId": "mySonarQube",
        "projectKey": "the-project-key-on-sonarqube"
    }
   }
   ```
