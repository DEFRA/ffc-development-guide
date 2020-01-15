# Overview
All technology choices are in line with Defra Digital’s Common Technology Choices (CTC).  A copy of which is available on SharePoint https://defra.sharepoint.com/sites/def-ddts-ioc/pdce/Cloud%20Ecosystem%20Wiki/Common%20Technology%20Choices%20V.01.aspx

These choice have been used as a baseline to help decide which technologies to build services with.  Where there are gaps in the CTC not covering FFC needs we have aligned ourselves to the architectural vision, Cloud Service Centre recognised patterns and lessons learned from other digital teams.  

FFC Test Lead has been involved in these discussions.

## Microservice Development
- Node.js
- Hapi.js

## Databases
### Relational
- PostgreSQL

### NoSQL
All NoSQL databases will use the Mongo driver as this is the only driver compatible with PaaS services in both Azure and AWS

- CosmosDB for NoSQL databases host in Azure
- DocumentDB (with MongoDB compatibility) for NoSQL databases hosted in AWS

## Infrastructure Building 
- Terraform

## Source Code Repository 
- GitHub (public)
- GitLab (private)

## Code Build Tools 
- Node Package Manager (NPM)

## CI/CD Pipelines 
- Jenkins (Groovy syntax)

## Repository Manager 
- JFrog Artifactory (TBC)

## Container Orchestration
- Docker
- Docker Compose
- Kubernetes

## Code Quality 
- SonarQube

## Testing
### Unit
- Jest

### Functional
- Cypress

### Compatibility
- Selenium
- BrowserStack

### Performance
- JMeter

### Security
- Snyk
- OWASP Zap

### Acceptance
- Cypress
- Cucumber

### Accessibility
- Pa11y
