# Configuration management

Non sensitive application configuration data is persisted in [Azure Application Configuration](https://azure.microsoft.com/en-gb/services/app-configuration/).

Sensitive application data is persisted in [Azure Key Vault](https://azure.microsoft.com/en-gb/services/key-vault/) and referenced from Application Configuration.

One instance of Azure Key Vault and Azure Application Configuration will exist in every FFC Azure subscription.

The configuration keys used in Application Configuration must follow FFC naming convention below to ensure that CI and CD pipelines can retrieve values in a predictable and efficient way.

## Helm values in deployment

During deployment, our CI or CD pipeline will read the `values.yaml` file in the Helm chart to identify all potential configuration values that may need to be sourced from Application Configuration and then matches each of them by key name.

For example, if a helm chart contains the below, then the key to be matched will be `container.port`.

```
container:
  port: 3000
```

If a key exists then the value, `3000` in the example, will be overwritten with the value in Application Configuration.

## Key naming standards

As all FFC teams will be sharing a single instance of Application Configuration in each subscription and that subscription may host multiple environments, we need to ensure our key naming convention is scalable, performant and avoids duplication where possible.

There are five accepted formats a key name can follow.  The examples all use the `container.port` key as a demonstration for simplicity.

### `common/key`

These are values that are consistent, regardless of service or environment. 

For example `common/container.port` would mean all services would all get the same value wherever they deploy.

The `common/` text is necessary due to the limitation the Azure CLI used in pipelines returns keys. 
Without the prefix there would be no way to only return these common items.

### `environment/key`

These are values that are environment specific, but not service specific.  

For example, `dev/container.port` would mean all services would get the same value when deploying to the `dev` environment.

### `service/common/key`

These are values that are service specific, but not environment specific.

For example, `ffc-demo/common/container.port` would mean all deployments belonging to the `ffc-demo` service would get the same value wherever they deploy.

As before the `/common/` text is necessary to ensure the Azure CLI returns only relevant values.

### `service/environment/key`

These are values that are service and environment specific.

For example, `ffc-demo/dev/container.port` would mean all deployments belonging to the `ffc-demo` service would get the same value when deploying to the `dev` environment.

### `pr/key`

These are only used for PR deployments in CI when you need to provide a different value for that PR deployment and still be able to provide individual microservie labels (see below).  They are also used by CI to obtain dynamic infrastructure values.

For example, `pr/container.port` would mean all PR deployments would get the same port value.

## Microservice specific values

When you want to provide a value depending on the specific microservice and not just the service you can add a different label to that key referncing the name of the microservice.

For example, if you have a key `ffc-demo/common/container.port` with an unlabelled value of `4000` and a value of `5000` labelled `ffc-demo-web`, then all `ffc-demo` services would get a value of `4000` except the `ffc-demo-web` microservice which would get `5000`.

## Order values are overridden

Within the CI and CD pipelines, the values will be sourced and overwritten in the following order.

1. `common/key`
2. `common/key` plus microservice label
3. `environment/key`
4. `environment/key` plus microservice label
5. `service/common/key`
6. `service/common/key` plus microservice label
7. `service/environment/key`
8. `service/environment/key` plus microservice label
9. `pr/key` (CI PR deployment only)
10. `pr/key` plus microservice label (CI PR deployment only)

## Deciding which convention to use

To improve performance in CI, keys should follow a convention that gives the narrowest scope possible.  

For example, if a value is only used for one service or will vary depending on the service then it should be scoped to either `service/common` or `service/enviornment`

Only values that are used by everyone should be scoped to `common/` or `environment/`

### Exceptions to the rule

The are some values that are exceptions to this rule because of dynamic infrastructure provisioning and related test exection in CI.

#### Ingress values

FFC Helm charts define their ingress resources like the below:

```
ingress:
  server:
  endpoint:
  class:
```

These values must use the hierarchy `environment/key` for example `dev/ingress.server` and use labels to separate each service's value.

#### Database values

FFC Helm charts define their database names like the below:

```
postgresService:
  postgresUser:
  postgresDb:
```

These values must use the hierarchy `environment/key` for example `dev/posgresService.postgresUser` and use labels to separate each service's value. 
