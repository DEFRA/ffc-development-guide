# Developing in a container

The FFC Architecture Vision prescribes a containerised microservice ecosystem.  Docker is the containerisation technology used within Defra.

Containers are lightweight and fast. One of their main benefits for developers is that it is simple to replicate an application’s environment and dependencies locally consistently. 

Crucially, they enable a workflow for your code that allows you to develop and test locally, push to upstream, and be confident that what you have built locally will work in CI and any environment.

The following guide has a strong focus on Node.js as it is the primary technology choice of FFC and Defra.  However, details of debugging .NET containers is included in [another guide](./debug-dotnet-container.md).

## Docker

All FFC microservices are built from supported Defra parent images for Node.js and .NET.  A `Dockerfile` will be included in each microservice repository containing a multi-stage build definition referencing these images.

All microservice repositories created from the [FFC Node template](https://github.com/DEFRA/ffc-template-node) will include this `Dockerfile` already configured.

## Docker Compose

[Docker Compose](https://docs.docker.com/compose/) is a tool for defining and running multi-container Docker applications using yaml configuration files.

All microservice repositories created from the [FFC Node template](https://github.com/DEFRA/ffc-template-node) include a pre-configured set of Docker Compose yaml files to support local development and testing as well as some being a prerequisite for CI capability.

An example Node.js `Dockerfile` showing a multi-stage build for both development and production.  Note that the `development` image runs the application in `watch` mode to support local development and testing, whilst `production` simply runs the application.

`development` is dependent on the local `package.json` including a watch script.  More on this below.

```
ARG PARENT_VERSION=1.2.9-node14.17.6
ARG PORT=3000
ARG PORT_DEBUG=9229

# Development
FROM defradigital/node-development:${PARENT_VERSION} AS development
ARG PARENT_VERSION
LABEL uk.gov.defra.ffc.parent-image=defradigital/node-development:${PARENT_VERSION}

ARG PORT
ARG PORT_DEBUG
ENV PORT ${PORT}
EXPOSE ${PORT} ${PORT_DEBUG}

COPY --chown=node:node package*.json ./
RUN npm install
COPY --chown=node:node . .
CMD [ "npm", "run", "start:watch" ]

# Production
FROM defradigital/node:${PARENT_VERSION} AS production
ARG PARENT_VERSION
LABEL uk.gov.defra.ffc.parent-image=defradigital/node:${PARENT_VERSION}

ARG PORT
ENV PORT ${PORT}
EXPOSE ${PORT}

COPY --from=development /home/node/app/ ./app/
COPY --from=development /home/node/package*.json ./
RUN npm ci
CMD [ "node", "app" ]
```

### `docker-compose.yaml` 
Used to define creation of the production image locally and in CI.  This file should include all configuration needed to create a clean production image.  Port and local volume mapping should be avoided in this file.

The template repository will set a `container_name` property in this file so that containers created have shorter and more predictable names to support local development.  However, if local scaling of container instances is required, then this property should be removed as container names will need to be dynamic in that scenario.

To avoid duplication, other dependent container images can be defined in this file such as PostgreSQL or Redis, but no volume or port bindings for those dependencies should be included.

### `docker-compose.override.yaml` 
Used to apply overrides to `docker-compose.yaml` to support local development.  This is where port and volume mappings should be declared.

If dependencies such as PostgreSQL or Redis are used, this is the file where volume and port bindings should be declared for those dependencies.

This image will build the `development` image which typically is the same as production but will run the code in `watch` mode so changes made to the code locally are automatically picked up in the container and restart the application.

#### Avoiding port conflicts
When binding container ports to localhost, it is important to consider any conflicts that may occur with other services developers may wish to run locally.

For example, if a service is made up of two microservices, both running on port `3000`.  Then both cannot be mapped to `localhost:3000` without a conflict.

In this scenario, to successfully run both services on the same device with port binding, one of the services should bind the container's port `3000` to a different localhost port.

The same consideration should be given to the debug port exposed to avoid a conflict on port `9229`, the default Node debug port.

```
# service 1 docker-compose.override.yaml
ports:
  - "3000:3000"
  - "9229:9229"

# service 2 docker-compose.override.yaml
ports:
  - "3001:3000"
  - "9230:9229"
```

This equally applies when binding dependency images such as PostgreSQL and Redis.

### `docker-compose.debug.yaml` 
Used to start application in debug mode.  This is only required if you wish the application to wait for the debugger before starting the application.  If you just wish to attach a debugger to an already running instance, the override file is sufficient.

### `docker-compose.test.yaml`
Used to run all tests in the repository.  This is a dependency of the FFC CI pipeline.  Port bindings should be avoided in this file to avoid conflicts between running builds.

### `docker-compose.test.watch.yaml`
Used as an override to `docker-compose.test.yaml` to run tests in `watch` mode to support Test Driven Development (TDD).  Changes to either application or test code will automatically trigger re-running of affected tests.

All Node.js FFC microservices use [Jest](https://jestjs.io/).  Jest has powerful capability to support multiple watch scenarios such as running individual tests, only tests affected by changes, only failed tests, filtering by regular expression as well as running the full suite.

In order to understand which code has changed, Jest uses the local `.git` directory.  This means when running tests in a container, the local `.git` folder must be mounted to the container in `docker-compose.watch.yaml`.

```
volumes:
  - ./.git:/home/node/.git
```

### `docker-compose.test.debug.yaml`
Used to run Jest in watch mode but support debugging of tests.  This allows developers to have all the capability of `watch` but with the added bonus of being able to attach a debugger.

Details of debugging tests are below.

## package.json scripts
To enable the capability provided by the above Docker Compose files, `package.json` needs to be configured to support the scripts referenced in the `command`.

Below is an extract of the default `package.json` file provided by [FFC Node template](https://github.com/DEFRA/ffc-template-node).

```
"scripts": {
    "pretest": "npm run test:lint",
    "test": "jest --runInBand --forceExit",
    "test:watch": "jest --coverage=false --onlyChanged --watch --runInBand",
    "test:debug": "node --inspect-brk=0.0.0.0 ./node_modules/jest/bin/jest.js --coverage=false --onlyChanged --watch --runInBand --no-cache",
    "test:lint": "standard",
    "start:watch": "nodemon --inspect=0.0.0.0 --ext js --legacy-watch app/index.js",
    "start:debug": "nodemon --inspect-brk=0.0.0.0 --ext js --legacy-watch app/index.js"
```

### `pretest`
This will automatically run before the `test` script and will lint all JavaScript files in according with StandardJs standards.

### `test`
This will run all Jest tests within the repository with no watch mode enabled and will output code coverage results on test completion.  This is primary used for CI, but can be run locally as a quick check of test status.

`--runInBand` will ensure that tests run sequentially rather than parallel.  Although this will result in slower running overall, it means that integration tests spanning containers have connections that are cleanly and predictably open and closed to avoid test disruption.

`--forceExit` will force Jest close a test with open connections 1 second after completion of the test.  Ideally this would not be needed, however in some scenarios Jest is unable to determine whether a Hapi server is still running even if it is cleanly shut down in the test.

### `test:watch`
This will run tests in `watch` mode and is the most commonly used by developers to support TDD.

`--coverage=false` - as typically only running a subset of tests, there is little value displaying a test coverage summary.  Disabling it also reduces lines written to the console to support developer focus.

`--onlyChanged` - start by only running tests that are affected by code changes.  Accuracy of this is dependent on the `.git` folder being mounted to the volume as described above as well as the folders containing test and application code.

`--watch` - will run tests in watch mode, so as code, either in application or test, is changed the tests are automatically re-run.  Developers have the option to change the behaviour of watch mode.

### `test:watch`
This will run tests in `watch` mode and is the most commonly used by developers to support TDD.

`--coverage=false` - as typically only running a subset of tests, there is little value displaying a test coverage summary.  Disabling it also reduces lines written to the console to support developer focus.

`--onlyChanged` - start by only running tests that are affected by code changes.  Accuracy of this is dependent on the `.git` folder being mounted to the volume as described above.

`--watch` - will run tests in watch mode, so as code, either in application or test, is changed the tests are automatically re-run.  Developers have the option to change the behaviour of watch mode.

### `test:debug`
This runs tests in `watch` mode and has all the same behaviour and running options as `test:watch`.  However, the key difference it this will wait for a debugger to be attached before starting test execution.  This enables developers to apply breakpoints in the test and application code to debug troublesome tests.

An example Visual Studio Code debugging profile to use this script is provided below.

`--no-cache` - Jest caches files between test runs which can result in some breakpoints not being hit.  This disables that behaviour.

### `test:lint`
Run linting with StandardJs only.

### `start:watch`
Starts the application in watch mode.  This is typically how developers will run all applications locally.  As code is changed, the running application in the container automatically identifies the changes and restarts the running application.

[Nodemon](https://www.npmjs.com/package/nodemon) is used to orchestrate restarting of the application.

This is dependent on the application code folder having a volume binding to the container.

### `start:debug`
This has the same behaviour as `start:watch` but like `test:debug` will wait for a debugger to be attached before executing any code.

## Convenience scripts
Repositories created from [FFC Node template](https://github.com/DEFRA/ffc-template-node) will include two convenience scripts to support developers easily running the application utilising the above setup.

### `./scripts/start`
Run the application using Docker Compose.  Typically this is just a simple abstraction over `docker-compose up` however, is can be extended to ensure container runs in a specific container network or run database migrations prior to starting the application for example.

### `./scripts/test`
Run tests.  Without any arguments provided will run the `test` script in `package.json`.

To ensure clean running, all test containers are recreated.  Note that this will not affect data persisted in development databases for example if the above setup is followed.

#### Optional arguments
`-h` - shows all available arguments.

`-w` - runs tests in watch mode using `test:watch` script

`-d` - runs tests in debug mode using `test:debug` script

## Debugging code running in a container
If the above setup is followed, then everything is in place to support debugging of applications and tests in containers.

Developers are free to use their own choice of IDE, however, all example debug configurations within this guide will assume Visual Studio Code is used.

These debug configurations should all be included in a `launch.json` file in the `.vscode` folder at the root of the repository.  This folder should be excluded from source control.

### Application debugging profiles
#### Attach to an already running container

```
{
  "name": "Docker: Attach",
  "type": "node",
  "request": "attach",
  "restart": true,
  "port": 9229,
  "remoteRoot": "/home/node",
  "skipFiles": [
    "<node_internals>/**",
    "**/node_modules/**"
  ]
}
```
This will attach to the node process exposed by the debug port.  Note that this uses the `localhost` port not the container port.  So if port `9229` is bound to a different port locally, then this value should be changed to match here.

`restart` - will ensure that as code is changed and the application restarted, the debugger is automatically reattached.

`remoteRoot` - must match the location of the code that matches the local workspace structure.  When using the Node.js Defra Docker base images the location will always be `home/node`.

`skipFiles` - an array of locations where debugging such skip.  Typically this would be internal Node.js code as well as those from third party npm modules.

#### Start an application in debug mode

```
{
      "name": "Docker: Attach Launch",
      "type": "node",
      "request": "attach",
      "remoteRoot": "/home/node",
      "restart": true,
      "port": 9229,
      "skipFiles": [
        "<node_internals>/**",
        "**/node_modules/**"
      ],
      "preLaunchTask": "compose-debug-up",
      "postDebugTask": "compose-debug-down"
    },
```

This will start a new container in debug mode using the `start:debug` `package.json` script.  The application will wait for a debugger before running any code.

This is dependent on `preLaunchTask` and `postDebugTask` being defined in a `.vscode/tasks.json` file.

An example of this is below.

```
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "compose-debug-up",
      "type": "shell",
      "command": "docker-compose -f docker-compose.yaml -f docker-compose.override.yaml -f docker-compose.debug.yaml up -d"
    },
    {
      "label": "compose-debug-down",
      "type": "shell",
      "command": "docker-compose -f docker-compose.yaml -f docker-compose.override.yaml -f docker-compose.debug.yaml down"
    }
  ]
}
```

### Test debugging
```
{
  "name": "Docker: Jest Attach",
  "type": "node",
  "request": "attach",
  "port": 9229,
  "restart": true,
  "timeout": 10000,
  "remoteRoot": "/home/node",
  "disableOptimisticBPs": true,
  "continueOnAttach": true,
  "skipFiles": [
    "<node_internals>/**",
    "**/node_modules/**"
  ]
}
```

This assumes that the `./script/test -d` command referenced above has already been run and the test suit is waiting for the debugger to attach. 
This profile will attach that debugger.

`disableOptimisticBPs` - this needs to be set as `true` as Jest takes a copy of all test files and uses these copies for execution.  If this is not disabled then this process can result in breakpoints not being mapped to their correct location.

`continueOnAttach` - instruct the pending test execution to continue once the debugger is attached.

## Other debugging profiles
A range of different debugging profiles can be found in [this repository](https://github.com/johnwatson484/vscode-debug-profiles-node) as well as a test application setup to the above standards to test them.

## Other useful Docker development guides
Defra has well documented [standards](https://github.com/DEFRA/software-development-standards/blob/master/standards/container_standards.md) and [guidance](https://github.com/DEFRA/software-development-standards/blob/master/guides/docker_guidance.md) on developing with containers, that provides further examples of good local development practice.
