# Postgres Database

## Create Managed Identity for your microservice

If not alreay configured add [Managed Identity](managed-identity.md) to your microservice

## Create Database

Request the creation of a database within your Azure Database for PostgreSQL service through your usual Cloud Services support channel:

The name of the database should be `ffc_<workstream>_<service>` (note here the use of underscores instead of the normal hyphen convention. Postgres hyphens require escapiing so underscores are preferred).

Create a database role that is bound to the managed identity for the microservice ([Azure guidence](https://docs.microsoft.com/en-us/azure/postgresql/howto-connect-with-managed-identity#creating-a-postgresql-user-for-your-managed-identity))

## Create database from Liquibase changelog

Make sure you have a Liquibase changelog defining the structure of your database available from the root of your microservice repoository in `changelog/db.changelog.xml`.

Guidence on creating a Liquibase changelog is outside of the scope of this guide, so please check current best practice with the FFC Platform Team.

## Update Docker Compose files to use Postgres environment variables and database migrations

`docker-compose.yaml`:

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

`docker-compose.override.yaml`:

```
ffc-<workstream>-<service>-postgres:
    ports:
      - "5432:5432"
```

Add the file `docker-compose.migrate.yaml`:

```
version: '3.8'

x-common-migration: &common-migration
  POSTGRES_HOST: ${POSTGRES_HOST:-ffc-<workstream>-<service>-postgres}
  SCHEMA_NAME: ${POSTGRES_SCHEMA_NAME:-public}
  SCHEMA_ROLE: ${POSTGRES_SCHEMA_ROLE:-postgres}
  SCHEMA_PASSWORD: ${POSTGRES_SCHEMA_PASSWORD:-ppp}
  SCHEMA_USERNAME: ${POSTGRES_SCHEMA_USERNAME:-postgres}

x-common-postgres: &common-postgres
  POSTGRES_PORT: 5432
  POSTGRES_DB: ${POSTGRES_DB:-ffc_<workstream>_<service>}
  POSTGRES_PASSWORD: ${POSTGRES_ADMIN_PASSWORD:-ppp}
  POSTGRES_USER: ${POSTGRES_ADMIN_USERNAME:-postgres}

services:
  database-up:
    image: liquibase/liquibase:3.10.x
    environment:
      << : *common-migration
      << : *common-postgres
    entrypoint: >
      sh -c "/scripts/migration/database-up"
    depends_on:
      - ffc-<workstream>-<service>-postgres
    volumes:
      - ./changelog/:/liquibase/changelog/
      - ./scripts/:/scripts/

    database-down:
    image: liquibase/liquibase:3.10.x
    environment:
      << : *common-migration
      << : *common-postgres
    entrypoint: >
      sh -c "/scripts/migration/database-down"
    depends_on:
      - ffc-<workstream>-<service>-postgres
    volumes:
      - ./changelog/:/liquibase/changelog/
      - ./scripts/:/scripts/

  ffc-<workstream>-<service>-postgres:
    image: postgres:11.4-alpine
    environment: *common-postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data: {}
```

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
