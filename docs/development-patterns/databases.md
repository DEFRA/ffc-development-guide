# Databases

PostgreSQL is the preferred database for microservices in the FCP Platform.  This guide describes the process for creating a database for a microservice and configuring the microservice to use it.

## Request creation of microservice database
CCoE are the only team with access to create database within your Azure Database for PostgreSQL in the Sandpit environment.  The name of the database should match the microservice repository name.  Microservices should not share a database.

For example, a microservice named `ffc-demo-claim-service` would have a database named `ffc_demo_claim_service`

*Note here the use of underscores instead of the normal hyphen convention. Postgres hyphens require escaping with double quote marks so underscores are preferred.*

## Request creation of microservice database role

Request Cloud Services to create a database role that is bound to the Managed Identity created for the microservice ([Azure guidence](https://docs.microsoft.com/en-us/azure/postgresql/howto-connect-with-managed-identity#creating-a-postgresql-user-for-your-managed-identity)), for example `ffc-snd-demo-claim-role`.

This identity must also be assigned to the Jenkins VM to ensure that Liquibase migrations can run.

## Create a Liquibase changelog

The FCP Platform CI and deployment pipelines support database migrations using [Liquibase](https://www.liquibase.org/).

Create a Liquibase changelog defining the structure of your database available from the root of your microservice repository in `changelog/db.changelog.xml`.

Guidance on creating a Liquibase changelog is outside of the scope of this guide, so please check current best practice with the FCP Platform Team.

## Update Docker Compose files to use Postgres service and environment variables

Update `docker-compose.yaml`, `docker-compose.override.yaml`, and `docker-compose.test.yaml` to include a Postgres service and add Postgres environment variables to the microservice.

An example Postgres service:

```yaml
services:
  # Microservice definition here
  ffc-<workstream>-<service>-postgres:
    image: postgres:11.4-alpine
    environment:
      POSTGRES_DB: ffc_<workstream>_<service>
      POSTGRES_PASSWORD: ppp
      POSTGRES_USER: postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data: {}
```

Add dependency on the Postgres service and environment variables the microservice `services` definition:

```yaml
services:
  # Microservice definition here
    depends_on:
      - ffc-<workstream>-<service>-postgres
    environment:
      POSTGRES_DB: ffc_<workstream>_<service>
      POSTGRES_PASSWORD: ppp
      POSTGRES_USER: postgres
      POSTGRES_HOST: ffc-<workstream>-<service>-postgres
      POSTGRES_PORT: 5432
      POSTGRES_SCHEMA_NAME: public
```

Replace `<workstream>` and `<service>` as per naming convention described above.

## Add Docker Compose files to run Liquibase migrations

Add a `docker-compose.migrate.yaml` to the root of the microservice based on [this example](https://github.com/DEFRA/ffc-demo-claim-service/blob/master/docker-compose.migrate.yaml).

## Update microservice Helm chart

Update the Helm chart values file (`helm/<REPO_NAME>/values.yaml`) with default values for the Postgres service:

```yaml
postgresService:
  postgresDb: ffc_<workstream>_<service>
  postgresExternalName:
  postgresHost: ffc-<workstream>-<service>-postgres
  postgresPort: 5432
  postgresSchema: public
  postgresUser: postgres
```

replacing `<workstream>` and `<service>` as per naming convention described above.

### Update ConfigMap

Update the `ConfigMap` template of the Helm Chart (`helm/<REPO_NAME>/templates/config-map.yaml`) to include the environment variables for the Postgres database:

```yaml
POSTGRES_DB: {{ quote .Values.postgresService.postgresDb }}
POSTGRES_HOST: {{ quote .Values.postgresService.postgresHost }}
POSTGRES_PORT: {{ quote .Values.postgresService.postgresPort }}
POSTGRES_SCHEMA_NAME: {{ quote .Values.postgresService.postgresSchema }}
```

### Create/Update the container Secret

Create (or update) the Secret template in `helm/<REPO_NAME>/templates/container-secret.yaml`:

```yaml
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

replacing `<workstream>` and `<service>` as per naming convention described above.

## Add Liquibase migration scripts

Create the following resources from the root level of your repository
* `scripts/migration/database-down`

```bash
#!/bin/bash
echo "db update on $POSTGRES_HOST $SCHEMA_NAME as $SCHEMA_USERNAME"
/scripts/postgres-wait && /liquibase/liquibase \
--driver=org.postgresql.Driver \
--changeLogFile=/changelog/db.changelog.xml \
--url=jdbc:postgresql://$POSTGRES_HOST:$POSTGRES_PORT/$POSTGRES_DB \
--username="$SCHEMA_USERNAME" --password="$SCHEMA_PASSWORD" --defaultSchemaName="$SCHEMA_NAME" \
rollback v0.0.0
```

* `scripts/migration/database-up`

```bash
#!/bin/bash
echo "db update on $POSTGRES_HOST $SCHEMA_NAME as $SCHEMA_USERNAME"
/scripts/postgres-wait && /liquibase/liquibase \
--driver=org.postgresql.Driver \
--changeLogFile=/changelog/db.changelog.xml \
--url=jdbc:postgresql://$POSTGRES_HOST:$POSTGRES_PORT/$POSTGRES_DB \
--username="$SCHEMA_USERNAME" --password="$SCHEMA_PASSWORD" --defaultSchemaName="$SCHEMA_NAME" \
update

```

* `scripts/postgres-wait`

```bash
#!/bin/bash
echo "waiting for postgres $POSTGRES_HOST:$POSTGRES_PORT"
response=''
max_tries=15
count=0
while :
do
  ((count++))
  if [ "$count" -ge "$max_tries" ]; then
		echo "postgres did not respond in time"
		exit 1
	fi
  response=$(wget -SO- -T 1 -t 1 http://$POSTGRES_HOST:$POSTGRES_PORT 2>&1 | grep 'No data received')
  if [ "$response" ]; then
    echo "postgres server available"
    break
  fi
  printf '.'
  sleep 2
done
echo postgres started
```

## Add values to Azure Key Vault and App Configuration

Azure Key Vault is used to store the Postgres username and Azure App Configuration is used to stores values required by the Jenkins CI pipelines.

Create the following secret in Azure Key Vault via the Azure Portal:

* **Name**: `snd-postgres<workstream><service>User`; **Value**: `<managed-identity>@<azure-postgres-instance>` (e.g. `ffc-snd-demo-web-role@mypostgresserver`)

Create the following entries in Azure App Configuraiton via the Azure Portal:

1. A key-value entry where **Key**: `<environment>/postgresService.postgresDb` (e.g. `dev/postgresService.postgresDb`); **Value**: `ffc_<workstream>_<service>` (e.g. `ffc_demo_claim_service`); **Label**: `<REPO_NAME>` (e.g. `ffc-demo-claim-service`)

2. A Key Vault reference entry where **Key**: `dev/postgresService.postgresUser`; **Key Vault Secret Key**: `dev-postgres<workstream><service>User`; **Label**: `<REPO_NAME>` (e.g. `ffc-demo-claim-service`)

where `<workstream>` and `<service>` refer to those parts of the queue name described above.

**Note** in environments beyond Sandpit, Azure DevOps will provision databases suffixed with the target environment.  The values should be ammended accordingly. e.g. `ffc_demo_claim_service_dev`

## Add database code to the microservice

Update your microservice code using the relevant Azure authentication SDKs for your language.

Patterns for using a Postgres database in microservice code are outside of the scope of this guide. An example is shown below for a Node.js microservice, but please check current best practice with the FCP Platform Team.

### Node.js example

Install the Azure Authentication SDK NPM package: `npm install @azure/identity`.

With the Managed Identity bound to your microservice in the Kubernetes cluster (following the guidence above), you can then access the database using the username `<managed-identity>@<azure-postgres-instance>` (e.g. `ffc-snd-demo-web-role@mypostgresserver`) and an access token as the password:

```javascript
async function example() {
  const { DefaultAzureCredential } = require('@azure/identity')
  const credential = new DefaultAzureCredential()
  const accessToken = await credential.getToken('https://ossrdbms-aad.database.windows.net', { requestOptions: { timeout: 1000 } })
  cfg.password = accessToken.token
}
```
