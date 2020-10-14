# Postgres Database

This guide describes how to add a Postgres database to a microservice running on the FFC Platform Kubernetes cluster.

## Create Managed Identity for your microservice

If not alreay configured add [Managed Identity](managed-identity.md) to your microservice

## Request creation of microservice database

Request Cloud Services through your usual support channel to create a database within your Azure Database for PostgreSQL following the naming convention:

```
ffc_<workstream>_<service>
```

for example `ffc_demo_claim`.

*Note here the use of underscores instead of the normal hyphen convention. Postgres hyphens require escaping with double quote marks so underscores are preferred.*

## Request creation of microservice database role

Request Cloud Services to create a database role that is bound to the Managed Identity created for the microservice ([Azure guidence](https://docs.microsoft.com/en-us/azure/postgresql/howto-connect-with-managed-identity#creating-a-postgresql-user-for-your-managed-identity)), for example `ffc-snd-demo-claim-role`.

## Create a Liquibase changelog

The FFC Platfrom CI and deployment pipelines support database migrations using [Liquibase](https://www.liquibase.org/).

Create a Liquibase changelog defining the structure of your database available from the root of your microservice repoository in `changelog/db.changelog.xml`.

Guidence on creating a Liquibase changelog is outside of the scope of this guide, so please check current best practice with the FFC Platform Team.

## Update Docker Compose files to use Postgres service and environment variables

Add the following configuration to the microservice `docker-compose.yaml`:

```
    depends_on:
      - ffc-<workstream>-<service>-postgres
    environment:
      POSTGRES_DB: ffc_<workstream>_<service>
      POSTGRES_PASSWORD: ppp
      POSTGRES_USER: postgres
      POSTGRES_HOST: ffc-<workstream>-<service>-postgres
      POSTGRES_PORT: 5432
      POSTGRES_SCHEMA_NAME: public

  ffc-<workstream>-<service>-postgres:
    image: postgres:11.4-alpine
    environment:
      POSTGRES_DB: ffc_<workstream>_<service>
      POSTGRES_PASSWORD: ppp
      POSTGRES_USER: postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data: {}
```

Replace `<workstrem>` and `<service>` as per naming convention described above.

## Update Docker Compose Override files to use Postgres service

Add the following configuration to the microservice `docker-compose.override.yaml`:

```
  ffc-<workstream>-<service>-postgres:
    ports:
      - "5432:5432"
```

Replace `<workstrem>` and `<service>` as per naming convention described above.

## Add Docker Compose files to run Liquibase migrations

Add a `docker-compose.migrate.yaml` to the root of the microservice based on [the template provided in resources](../../resources/docker-compose.migrate.yaml).

Replace `<workstrem>` and `<service>` as per naming convention described above.

## Update microservice Helm chart

### Create a Postgres Service

Create a `Service` Kubernetes template for Postgres in `helm/<REPO_NAME>/templates/postgres-service.yaml`:

```
{{- if .Values.postgresService.postgresExternalName }}
{{- include "ffc-helm-library.postgres-service" (list . "<REPO_NAME>.postgres-service") -}}
{{- end }}
{{- define "<REPO_NAME>.postgres-service" -}}
{{- end -}}
```

replacing `<REPO_NAME>` with the git repository name.

Update the Helm chart values file (`helm/<REPO_NAME>/values.yaml`) with default values for the Postgres service:

```
postgresService:
  postgresDb: ffc_<workstream>_<service>
  postgresExternalName:
  postgresHost: ffc-<workstream>-<service>-postgres
  postgresPort: 5432
  postgresSchema: public
  postgresUser: postgres
```

replacing `<workstrem>` and `<service>` as per naming convention described above.

### Update ConfigMap

Update the `ConfigMap` template of the Helm Chart (`helm/<REPO_NAME>/templates/config-map.yaml`) to include the environment variables for the Postgres database:

```
POSTGRES_DB: {{ quote .Values.postgresService.postgresDb }}
POSTGRES_HOST: {{ quote .Values.postgresService.postgresHost }}
POSTGRES_PORT: {{ quote .Values.postgresService.postgresPort }}
POSTGRES_SCHEMA_NAME: {{ quote .Values.postgresService.postgresSchema }}
```

### Create/Update the container Secret

Create (or update) the Secret template in `helm/<REPO_NAME>/templates/container-secret.yaml`:

```
{{- include "ffc-helm-library.container-secret" (list . "<REPO_NAME>.container-secret") -}}
{{- define "<REPO_NAME>.container-secret" -}}
stringData:
  POSTGRES_USER: {{ .Values.postgresService.postgresUser | quote }}
{{- end -}}
```

replacing `<REPO_NAME>` with the git repository name.

Update the Helm chart values file (`helm/<REPO_NAME>/values.yaml`) with a name for the Secret:

```
containerSecret:
  name: ffc-<workstream>-<service>-container-secret
  type: Opaque
```

replacing `<workstrem>` and `<service>` as per naming convention described above.

## Add Liquibase migration scripts

Copy the [scripts from resources](../../resources/scripts) to create the following scripts at the root of your microservice:
* `scripts/migration/database-down`
* `scripts/migration/database-up`
* `scripts/postgres-wait`

## Add values to Azure Key Vault and App Configuration

Azure Key Vault is used to store the Postgres username and Azure App Configuration is used to stores values required by the Jenkins CI pipelines.

Create the following secret in Azure Key Vault via the Azure Portal:

* **Name**: `dev-postgres<workstream><service>User`; **Value**: `<managed-identity>@<azure-postgres-instance>` (e.g. `ffc-snd-demo-web-role@mypostgresserver`)

Create the following entries in Azure App Configuraiton via the Azure Portal:

1. A key-value entry where **Key**: `dev/postgresService.postgresDb`; **Value**: `ffc_<workstream>_<service>`; **Label**: `<REPO_NAME>` (e.g. `ffc-demo-claim-service`)

2. A Key Vault reference entry where **Key**: `dev/postgresService.postgresUser`; **Key Vault Secret Key**: `dev-postgres<workstream><service>User`; **Label**: `<REPO_NAME>` (e.g. `ffc-demo-claim-service`)

where `<workstream>` and `<service>` refer to those parts of the queue name described above.

## Add database code to the microservice

Update your microservice code using the relevant Azure authentication SDKs for your language.

Patterns for using a Postgres database in microserive code are outside of the scope of this guide. An example is shown below for a Node.js microservice, but please check current best practice with the FFC Platform Team.

### Node.js example

Install the Azure Authentication SDK NPM package: `npm install @azure/ms-rest-nodeauth`.

With the Managed Identity bound to your microservice in the Kubernetes cluster (following the guidence above), you can then access the database using the username `<managed-identity>@<azure-postgres-instance>` (e.g. `ffc-snd-demo-web-role@mypostgresserver`) and an access token as the password:

```
async function example() {
  const auth = require('@azure/ms-rest-nodeauth')
  const credentials = await auth.loginWithVmMSI({ resource: 'https://ossrdbms-aad.database.windows.net' })
  const databasePassword = await credentials.getToken()

  // Use databasePassword along with Postgres role bound to Managed Identity to authenticate to your database
}
```
