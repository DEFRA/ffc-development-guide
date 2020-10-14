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

## Helm Chart

Create Postgres template (`helm/<REPO_NAME>/templates/postgres-service.yaml`):

```
{{- if .Values.postgresService.postgresExternalName }}
{{- include "ffc-helm-library.postgres-service" (list . "<REPO_NAME>.postgres-service") -}}
{{- end }}
{{- define "<REPO_NAME>.postgres-service" -}}
{{- end -}}
```

Update the ConfigMap template of the Helm Chart (`helm/<REPO_NAME>/templates/config-map.yaml`) to include the environment variables for the Postgres database:

```
POSTGRES_DB: {{ quote .Values.postgresService.postgresDb }}
POSTGRES_HOST: {{ quote .Values.postgresService.postgresHost }}
POSTGRES_PORT: {{ quote .Values.postgresService.postgresPort }}
POSTGRES_SCHEMA_NAME: {{ quote .Values.postgresService.postgresSchema }}
```

Then create default values for these in the `postgres` section of the Helm Chart values file (`helm/<REPO_NAME>/values.yaml`):

```
postgresService:
  postgresDb: ffc_<workstream>_<service>
  postgresExternalName:
  postgresHost: ffc-<workstream>-<service>-postgres
  postgresPort: 5432
  postgresSchema: public
  postgresUser: postgres
```

Add Secret template (`helm/<REPO_NAME>/templates/container-secret.yaml`)

```
{{- include "ffc-helm-library.container-secret" (list . "<REPO_NAME>.container-secret") -}}
{{- define "<REPO_NAME>.container-secret" -}}
stringData:
  POSTGRES_USER: {{ .Values.postgresService.postgresUser | quote }}
{{- end -}}
```

and update `values.yaml`:

```
containerSecret:
  name: ffc-<workstream>-<service>-container-secret
  type: Opaque
```

## Add scripts

`scripts/migration/database-down`
`scripts/migration/database-up`
`scripts/postgres-wait`

## Add database access code to the microservice (Node.js example)

Install Azure Authentication NPM package: `npm install @azure/ms-rest-nodeauth`.

With the Managed Identity bound to your microservice in the Kubernetes cluster, you can then access the database using the username `<managed-identity>@<azure-postgres-instance>` (e.g. `ffc-snd-demo-web-role@mypostgresserver` and an access token as the password e.g.:

```
const auth = require('@azure/ms-rest-nodeauth')
const credentials = await auth.loginWithVmMSI({ resource: 'https://ossrdbms-aad.database.windows.net' })
const databasePassword = await credentials.getToken()
```

Patterns for using a Postgres database are outside of the scope of this guide, but please check current best practice with the FFC Platform Team.
