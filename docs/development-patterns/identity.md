# Identity

Managed Identities should be used to authenticate applications with PaaS resources in Azure.

Credentials such as connection strings and passwords should be avoided where possible to avoid the need for secret management and rotation.

Managed identities can be created by updating the [Platform repository](https://dev.azure.com/defragovuk/DEFRA-FFC/_git/DEFRA-FFC-PLATFORM) and running the [appropriate pipeline](https://dev.azure.com/defragovuk/DEFRA-FFC/_build?definitionId=2752).

A Managed Identity should correspond to an application and an application should only have one identity.

## Create a Managed Identity (Sandpit only)

In Sandpit, Managed Identities must be created separately.

The Managed Identity should adhere to the following naming convention:

`ffc-<cloud-environment>-<workstream>-<service>-role`

for example `ffc-snd-demo-web-role`.

Once created, Azure Role Assignments can be added for the Managed Identity to access other Azure resources.

## Update Helm chart

The FCP Platform uses [Azure Active Directory Pod Identity](https://github.com/Azure/aad-pod-identity) to bind the Managed Identity to the Pod within AKS.

The enable this, add and configure the `AzureIdentity` and `AzureIdentityBinding` Kubernetes templates from the [ffc-helm-library](https://github.com/DEFRA/ffc-helm-library) to your Helm Chart (`helm/<REPO_NAME>/templates/`) [following the Helm Library guidance](https://github.com/DEFRA/ffc-helm-library#azure-identity-template).

> Note the `usePodIdentity` value in the `values.yaml` file should be set to `true`.

## Add Managed Identity values to Azure App Configuration

In order for For your newly created Managed Identity to be correctly injected, the appropriate configuration needs to be passed to the Helm chart at deployment time.

`azureIdentity.clientId` and `azureIdentity.resourceId`.

For Sandpit, these values can be obtained from the created Managed Identity and must be added to Azure App Configuration manually following the [configuration guidance](configuration.md).  

For other environments, these values are added the the [Platform repository](https://dev.azure.com/defragovuk/DEFRA-FFC/_git/DEFRA-FFC-PLATFORM).  However, rather than adding the actual values, tokenised values can be added following the format `<managed-identity-name>-clientId` and `<managed-identity-name>-resourceId`.
