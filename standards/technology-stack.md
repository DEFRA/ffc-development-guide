# Technology Stack
All technology choices are in line with Defra Digital’s Common Technology Choices (CTC). A copy of which is available on SharePoint https://defra.sharepoint.com/sites/def-ddts-ioc/pdce/Cloud%20Ecosystem%20Wiki/Common%20Technology%20Choices%20V.01.aspx

These choice have been used as a baseline to help decide which technologies to build services with. Where there are gaps in the CTC not covering FFC needs we have aligned ourselves to the architectural vision, Cloud Centre of Excellence recognised patterns and lessons learned from other digital teams.

When developing services, teams must use the following technology stack.  Where a new need is not covered by the below technology stack, collaboration between FFC architecture and the Tools Authority must take place to agree how to fill the gap.

## Microservice Development
- Node.js (with Hapi.js)
- npm
- .NET
- Nuget
- Docker
- Docker Compose
- Helm

## CI/CD
- Jenkins (Groovy syntax)
- ARM templates
- Azure CLI
- Azure DevOps

## Testing
- Jest
- NUnit
- Pact
- Web Driver IO
- Cucumber
- Selenium
- BrowserStack
- JMeter
- Snyk
- OWASP Zap
- Pa11y
- WAVE
- Anchore Engine
- Aqua Trivy

## Cloud (private)
- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Azure PostgreSQL
- Azure Service Bus
- Azure Application Configuration
- Azure Key Vault
- AAD Pod Identity
- Application Insights
- Azure Repos

## Cloud (public)
- GitHub
- SonarCloud
- DockerHub
- BrowserStack
- npm
- NuGet
