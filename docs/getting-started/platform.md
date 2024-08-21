# Platform

## FCP Platform

To support rapid, highly assured delivery, the FCP Platform engineering team has delivered common Azure environments, PaaS components, delivery pipelines and supporting tools.

These are collectively referred to as the `FCP Platform`.

All FCP developed services are deployed to the FCP Platform.  

Guidance for use of the FCP Platform is hosted within this repository.

### Reference architecture

![Reference architecture](../img/fcp-platform-reference-architecture.png)

### Environments

#### Sandpit

The `Sandpit` environment is the first deployment environment for new services following merge to the `main` branch.  It also hosts dynamic feature branch deployments.

It can also used for testing and experimentation.  Developers have a high level of access to the Sandpit environment to create and destroy resources as required.

Unlike all subsequent environments, provisioning of Azure resources such as Managed Identities, PostgreSQL is not automated.  Teams must create their own resources in this environment using the guides in this repository.

Although the majority of patterns between Sandpit are the same, a key difference is the use of [Azure App Configuration](https://docs.microsoft.com/en-us/azure/azure-app-configuration/overview) for configuration management.

The reason for this is that originally all environments used Azure App Configuration, however automation of configuration from an [Azure repo](https://dev.azure.com/defragovuk/DEFRA-FFC/_git/DEFRA-FFC-PLATFORM) has replaced this in all other environments.

#### Development

The `Development` environment is the first automation only deployment environment.  Different teams utilise it for many purposes such as demonstrations and testing.

Developers have full access to this environment, but cannot directly create or destroy resources.  Instead, they must use the automation provided to deploy their services.

#### Test

The `Test` environment is used for more formal testing and assurance activities such as integration testing and user acceptance testing.

> Note: FCP principles are to apply a "shift left" approach to testing where as much testing as possible is done during development prior to merging to `main`, as well as a principle of deploying frequent small changes to `Production`.
> Therefore, it is not expected that changes build up in `Test` ahead of a large scale deployment to higher environments.  For incomplete features or those awaiting wider scale testing, feature toggles are used to disable new behaviour and not block deployment through to `Production`.

Developers have full access to this environment, but cannot directly create or destroy resources.  Instead, they must use the automation provided to deploy their services.

#### Pre-Production

The `Pre-Production` environment is used as a staging area prior to deployment to `Production`.  Whilst some testing activities can be performed here, it is recommended to use `Test` for most testing activities as access to this environment is more restricted and requires Security Clearance.

Developers with Security Clearance can request read only access to this environment.

#### Production

The `Production` environment is the live environment for the service.  It is the only environment that is accessible to the public.

Developers with Security Clearance can request read only access to this environment.

## Other Defra Platforms

Since launch of the FCP Platform, Defra has subsequently created two Defra wide platform initiatives.

### Azure Development Platform (ADP)

ADP is essentially an iteration of the FCP Platform, but takes into account lessons learned throughout the lifetime of the FCP Platform to provide a better experience for teams utilising it.

Guidance for use of ADP is hosted within the [ADP Documentation](https://defra.github.io/adp-documentation/) and not within these pages.

### Core Development Platform (CDP)

CDP hosts Node.js and .NET services on [AWS Fargate](https://aws.amazon.com/fargate/) with support for MongoDb databases.

Guidance for use of CDP is hosted with the [CDP Documentation](https://github.com/DEFRA/cdp-documentation) and not within these pages.

### Choosing a Platform

> **Note** this position is currently under review pending confirmation of wider Defra platform strategy.

When starting a new project, the choice of platform should be made in consultation with the FCP Architecture team.

The default position is to use the existing FCP Platform.
