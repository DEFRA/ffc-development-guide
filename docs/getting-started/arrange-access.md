# Arrange access

Developers will need access to several resources and communication channels in order to be productive.

Access to some resources can take time to arrange, so it's important to start this process as soon as possible.

## Microsoft Teams

Microsoft Teams is Defra's primary collaboration tool.  All meetings should be arranged in Teams.

External suppliers can join Defra Teams meetings with their own email address if they do not have a Defra account.

## Slack

Slack is used for collaboration and support across all of Defra's digital services.

### `defra-digital.slack.com`

This is Defra's primary Slack workspace for digital delivery.

Whilst not essential to be part of this workspace, it may be beneficial for cross Defra collaboration.

Anyone with a `defra.gov.uk` or an ALB email address can join the Defra Digital Slack workspace automatically.

For those with other email addresses, a request should be raised in the `#slack-support` channel by someone with access.

Developers are recommended to join the following channels:

#### `#development`

General development discussion and support across all of Defra's digital services.

#### `#ask-principal-developers`

Ask for advice from Defra's [Principal Developers](https://ddat-capability-framework.service.gov.uk/role/software-developer#principal-developer).

#### `#github-support`

For support with GitHub including managing permissions.

#### `#sonar-support`

For support with SonarCloud including managing permissions.

### `defra-digital-team.slack.com`

This secondary Defra Digital Slack workspace is used for collaboration and support across all of Defra's digital services.

This is a paid instance so has additional collaboration features.

Developers are recommended to join the following channels:

#### `#fcp-platform-support`

Support and collaboration across all services using the FCP Platform.  It is also where the FCP Platform team will post updates and announcements.

#### `#cdp-support`

Support and collaboration across all services using the Core Data Platform.  It is also where the CDP team will post updates and announcements.

#### `#software-development`

General development discussion and support across all of Defra's digital services.

## Azure AD administrative account

Access to Azure and AWS cloud environments are only accessible with an administrative account setup in Microsoft Entra by CCoE.

These accounts follow the naming convention `a-<initials>.<surname>@defra.onmicrosoft.com` or `<initials>.<surname>@defra.onmicrosoft.com` and are a requirement for both FCP Platform and CDP.

The must be requested from ServiceNow under the catalogue item `Cloud Accounts` (subheading `M365/AzureAD Platform Admin - Cloud Account service request`)

The comments should make clear that the request is for a `a-` Microsoft administrative account for Azure Portal.

> Accounts without the `a-` prefix can be created but cannot be used for access to Production resources. 

## OpenVPN

Access to cloud resources from non-Defra supplied devices is restricted to VPN access only and requires the installation of the OpenVPN client.

If using FCP Platform, a request should be raised in ServiceNow for CCoE to provide access to OpenVPN and all FCP networks.

If using CDP, then the request should be made in the `#cdp-support` channel.

## Access to the Azure Portal

Once the account has been created, the user will be able to access the Azure Portal.

If using the FCP Platform, a request should be raised in ServiceNow for CCoE to provide access to the FCP Azure subscriptions.

FCP environments are split across three Azure tenants, `DefraCloudDev`, `DefraCloudPreProd` and `DefraCloudProd`.

If a developer has Security Clearance, they can request access to the `DefraCloudPreProd` and `DefraCloudProd` tenants.  Otherwise they will only be able to access the Development tenant.  The request should make clear which environments the developer requires access to.  

> Evidence of Security Clearance will be requested by CCoE prior to completing the request

If using CDP, then no direct access to the AWS portal is required.  Instead, the CDP portal should be used to manage resources.

## Access to Azure Kubernetes Service (AKS)

As AKS is the primary compute hosting service for the FCP Platform, developers using it will need to be added to the appropriate Kubernetes Cluster role.  This will allow access to Kubernetes using client tools such as `kubectl`.

As with the Azure Portal, developers can only be added to the PreProduction and Production clusters if they have Security Clearance.

A read only role, is all that is permitted for developers in PreProduction and Production.

A request should be raised in ServiceNow for CCoE to provide access to the FCP AKS clusters.

## Jenkins

Jenkins is used for Continuous Integration pipelines on the FCP Platform.  Developers will need a `@dtz.local` account to access Jenkins.

[Jenkins](https://jenkins-ffc.azure.defra.cloud/) is hosted on an Azure Virtual Machine and can be accessed on the Defra network or via OpenVPN.

A request should be raised in ServiceNow for CCoE to provide access to Jenkins.

## Snyk

Snyk is used for security scanning of code dependencies.  

An [FCP organisation](https://app.snyk.io/org/defra-ffc) has been created in Snyk and developers will need to be added to it.

Contact the Platform team via the `#fcp-platform-support` channel to arrange access.

> CDP CI pipeline does not include Snyk integration by default, but teams can extend their GitHub actions to integrate with the FCP Snyk organisation.

## GitHub

GitHub is Defra's strategic source control tool and **all** code should be hosted in the [Defra](https://github.com/DEFRA) organisation.

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

Whilst ADO Wiki's are preferred, significant content exists in [Defra Confluence](https://eaflood.atlassian.net/).

New licenses are often challenging to obtain, but should be requested through the `defra-digital.slack.com` Slack workspace in the `#jira-support` channel.

## CDP Portal

CDP has it's own [onboarding process](https://portal.cdp-int.defra.cloud/documentation/onboarding/onboarding-process.md) that teams should follow.
