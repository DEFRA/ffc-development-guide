# Technology Stack
All technology choices are in line with Defra Digital’s Common Technology Choices (CTC). A copy of which is available on SharePoint https://defra.sharepoint.com/sites/def-ddts-ioc/pdce/Cloud%20Ecosystem%20Wiki/Common%20Technology%20Choices%20V.01.aspx

These choice have been used as a baseline to help decide which technologies to build services with. Where there are gaps in the CTC not covering FFC needs we have aligned ourselves to the architectural vision, Cloud Service Centre recognised patterns and lessons learned from other digital teams.

When developing services, teams must use the following technology stack.

## Microservice Development
- Node.js (with Hapi.js)
- npm
- .NET Core
- Nuget
- Docker
- Docker Compose
- Helm

## CI
- Jenkins (Groovy syntax)
- ARM templates
- Azure CLI

## Testing
- Jest
- NUnit
- Pact
- Cypress
- Cucumber
- Selenium
- BrowserStack
- JMeter
- Snyk
- OWASP Zap
- Pa11y

## Cloud (private)
- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Azure PostgreSQL
- CosmosDB
- Azure Service Bus
- Application Configuration
- Azure Key Vault
- AAD Pod Identity
- Application Insights
- Azure Repos

## Cloud (public)
- GitHub
- SonarCloud
- DockerHub
