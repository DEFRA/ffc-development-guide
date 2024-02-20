# Arrange access

Developers will need access to several resources and communication channels in order to be productive.

Access to some resources can take time to arrange, so it's important to start this process as soon as possible.

## Azure AD administrative account

FCP environments are hosted on Azure.  Access to the Azure Portal is restricted to those with an Azure AD administrative account.

These accounts follow the naming convention `a-<initials>.<surname>@defra.onmicrosoft.com`.

The must be requested from ServiceNow under the catalogue item `O365/AzureAD Platform Admin`.

The comments should make clear that the request is for a `a-` Microsoft administrative account for Azure Portal.

### Access to the Azure Portal

Once the account has been created, the user will be able to access the Azure Portal.

A request should be raised in ServiceNow for CCoE to provide access to the FCP Azure subscriptions.

FCP environments are split across three Azure tenants, Development, PreProduction and Production.

If a developer has Security Clearance, they can request access to the PreProduction and Production tenants.  Otherwise they will only be able to access the Development tenant.  The request should make clear which environments the developer requires access to.  

> Evidence of Security Clearance will be requested by CCoE prior to completing the request

### Access to Azure Kubernetes Service (AKS)

As AKS is the primary compute hosting platform for FCP, developers will need to be added to the appropriate Kubernetes Cluster role.  This will allow access to Kubernetes using client tools such as `kubectl` or `Lens`.

As with the Azure Portal, developers can only be added to the PreProduction and Production clusters if they have Security Clearance.

A read only role, is all that is permitted for developers in PreProduction and Production.

A request should be raised in ServiceNow for CCoE to provide access to the FCP AKS clusters.

## Jenkins

Jenkins is used for CI/CD in FCP.  Developers will need a `.dtz` account to access Jenkins.

A request should be raised in ServiceNow for CCoE to provide access to Jenkins.

## Synk

Synk is used for security scanning of code dependencies.  

An FCP organisation has been created in Synk and developers will need to be added to it.

Contact one of Defra's Principal Developers to arrange access.

## GitHub

FCP uses GitHub for source control within the [Defra](https://github.com/DEFRA) organisation.

As per GDS standards, code is open source by default.

Developers will need to be added to the `FFC` GitHub team.

Two Factor Authentication (2FA) and commit signing are mandatory for all GitHub accounts.

Contact one of Defra's Principal Developers to arrange access.

## Azure DevOps

## Collaboration channels

