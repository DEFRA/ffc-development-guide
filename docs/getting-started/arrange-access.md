# Arrange access

Developers will need access to several resources and communication channels in order to be productive.

Access to some resources can take time to arrange, so it's important to start this process as soon as possible.

## Slack

### `defra-digital.slack.com`

The wider Defra Digital Slack workspace is used for collaboration and support across all of Defra's digital services.

Whilst not essential to be part of this workspace, it may be beneficial for cross Defra collaboration.

Anyone with a `defra.gov.uk` or an ALB email address can join the Defra Digital Slack workspace automatically.

For those with other email addresses, a request should be raised in the `#slack-support` channel by someone with access.

Developers are recommended to join the following channels:

#### `#development`

This channel is used for general development discussion and support across all of Defra's digital services.

> For FCP Platform or service specific support, the above Teams chats should be used instead.

#### `#github-support`

For support with GitHub including managing permissions.

#### `#sonar-support`

For support with SonarCloud including managing permissions.

### `defra-digital-team.slack.com`

This secondary Defra Digital Slack workspace is used for collaboration and support across all of Defra's digital services.

Developers are recommended to join the following channels:

#### `#fcp-platform-support`

This channel is used for support and collaboration across all services using the FCP Platform.  It is also where the FCP Platform team will post updates and announcements.

#### `#cdp-support`

This channel is used for support and collaboration across all services using the Core Data Platform.  It is also where the CDP team will post updates and announcements.

## Azure AD administrative account

FCP environments are hosted on Azure.  Access to the Azure Portal is restricted to those with an Azure AD administrative account.

These accounts follow the naming convention `a-<initials>.<surname>@defra.onmicrosoft.com` or `<initials>.<surname>@defra.onmicrosoft.com`.

The must be requested from ServiceNow under the catalogue item `O365/AzureAD Platform Admin`.

The comments should make clear that the request is for a `a-` Microsoft administrative account for Azure Portal.

### Access to the Azure Portal

Once the account has been created, the user will be able to access the Azure Portal.

A request should be raised in ServiceNow for CCoE to provide access to the FCP Azure subscriptions.

FCP environments are split across three Azure tenants, `DefraCloudDev`, `DefraCloudPreProd` and `DefraCloudProd`.

If a developer has Security Clearance, they can request access to the `DefraCloudPreProd` and `DefraCloudProd` tenants.  Otherwise they will only be able to access the Development tenant.  The request should make clear which environments the developer requires access to.  

> Evidence of Security Clearance will be requested by CCoE prior to completing the request

### OpenVPN

Access to cloud resources from devices is restricted to VPN access only and requires the installation of the OpenVPN client.

A request should be raised in ServiceNow for CCoE to provide access to OpenVPN and all FCP networks.

### Access to Azure Kubernetes Service (AKS)

As AKS is the primary compute hosting platform for FCP, developers will need to be added to the appropriate Kubernetes Cluster role.  This will allow access to Kubernetes using client tools such as `kubectl` or `Lens`.

As with the Azure Portal, developers can only be added to the PreProduction and Production clusters if they have Security Clearance.

A read only role, is all that is permitted for developers in PreProduction and Production.

A request should be raised in ServiceNow for CCoE to provide access to the FCP AKS clusters.

## Jenkins

Jenkins is used for Continuous Integration pipelines in FCP.  Developers will need a `@dtz.local` account to access Jenkins.

[Jenkins](https://jenkins-ffc.azure.defra.cloud/) is hosted on an Azure Virtual Machine and can be accessed on the Defra network or via OpenVPN.

A request should be raised in ServiceNow for CCoE to provide access to Jenkins.

## Snyk

Snyk is used for security scanning of code dependencies.  

An [FCP organisation](https://app.snyk.io/org/defra-ffc) has been created in Snyk and developers will need to be added to it.

Contact the Platform team via the `#fcp-platform-support` channel to arrange access.

## GitHub

FCP uses GitHub for source control within the [Defra](https://github.com/DEFRA) organisation.

As per GDS standards, code is open source by default.

Developers will need to be added to the `FFC` GitHub team.

Two Factor Authentication (2FA) and commit signing are mandatory for all GitHub accounts.

Contact one of Defra's GitHub admins via the `#github-support` Slack channel to arrange access.

## SonarCloud

SonarCloud is used for static code analysis during Continuous Integration pipelines in FCP.

FCP projects are hosted in the SonarCloud [Defra organisation](https://sonarcloud.io/organizations/defra). Within the Defra organisation an FCP group has been created to manage access to all FCP projects.

In order to be added to the FCP group:

1. Sign up to SonarCloud
1. Contact one of the Sonar admins via the `#sonar-support` Slack channel and request to be added to the `Farming and Countryside Programme` group, providing your email address used to sign in.

> Note: you must have signed into SonarCloud at least once before you can be added to a group.

## Azure DevOps (ADO)

Azure DevOps is used for Continuous Delivery pipelines in FCP.  ADO is also used for work item tracking and Wiki creation.

Developers will need to be added to the [DEFRA-FFC](https://dev.azure.com/defragovuk/DEFRA-FFC) project.

A request should be raised in ServiceNow for CCoE to provide access to ADO.

## SharePoint

SharePoint is Defra's primary document library.

An [FCP SharePoint site](https://defra.sharepoint.com/teams/Team1974/SitePages/Home.aspx) has been created.

Not all developers will need access to SharePoint and it may not be possible for external suppliers to be provided with access.

Should access be required, it should be request to an FCP Programme Delivery Manager.

## Confluence

Whilst ADO Wiki's are preferred, much historic content exists in [Defra Confluence](https://eaflood.atlassian.net/).

New licenses are often challenging to obtain, but should be requested through the `defra-digital.slack.com` Slack workspace in the `#jira-support` channel.
