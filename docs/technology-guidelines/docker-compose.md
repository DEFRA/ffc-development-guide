# Docker Compose

Docker compose is used for local development and in the [CI/CD pipeline](../cicd/index.md) to build docker containers and run tests.

## Use override files to reduce duplication

Additional settings can be applied to a docker compose file by using override files.

One use case is for testing - common settings can be put into the base `docker-compose.yaml` file, while changes to the command and image names can be placed in override files.

The below example demonstrates changing the command and container name for testing:

`docker-compose.yaml`

```
version: '3.4'
services:
  ffc-demo-service:
    build: .
    image: ffc-demo-service
    container_name: ffc-demo-service
    environment:
      DEMO_API: http://demo-api

volumes:
  node_modules: {}

```

`docker-compose.test.yaml`
```
version: '3.4'
services:
  ffc-demo-service:
    command: npm run test
    container_name: ffc-demo-service-test
```

The tests can be run by providing the override file as a file parameter:

`docker-compose run -f docker-compose.yaml -f docker-compose.test.yaml`

## Use projects to provide unique volumes and networks

To avoid conflicts when running different permutations of docker files, projects should be specified to segregate the volumes and networks.

This can be achieved with the `-p` switch when calling docker compose on the command line.

 i.e. to start the service

`docker-compose -p ffc-demo-service up -f docker-compose.yaml`

and to run the tests

`docker-compose -p ffc-demo-service-test run -f docker-compose.yaml -f docker-compose.test.yaml`

## Avoid conflicts with unique image names

The docker image name should be unique in override files if they may be run in simultaneously. 
If the `docker-compose.yaml` and the `docker-compose.test.yaml` had the same name conflicts will occur if tearing down and rebuilding the image happens while both images are in use.

## Use environment variables to guarantee unique projects and images

When running on a build server a combination of the `-p` switch and environment variables can be used to ensure each build and test have a unique project and images.

For example the following compose files can be started via:

`docker-compose -p ffc-demo-service-$PR_NO-$BUILD_NUMBER up -f docker-compose.yaml`

and 

`docker-compose -p ffc-demo-service-test-$PR_NO-$BUILD_NUMBER run -f docker-compose.yaml -f docker-compose.test.yaml`

to use PR and BUILD_NUMBER environment variables to isolate buiild tasks.

`docker-compose.yaml`

```
version: '3.4'
services:
  ffc-demo-service:
    build: .
    image: ffc-demo-service
    container_name: ffc-demo-service-${PR_NO}-${BUILD_NUMBER}
    environment:
      DEMO_API: http://demo-api

volumes:
  node_modules: {}

```

`docker-compose.test.yaml`
```
version: '3.4'
services:
  ffc-demo-service:
    command: npm run test
    container_name: ffc-demo-service-test-${PR_NO}-${BUILD_NUMBER}
```
