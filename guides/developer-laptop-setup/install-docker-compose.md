# Install Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

## WSL users
When installing Docker Desktop, [Compose](https://docs.docker.com/compose/cli-command/) is automatically added to WSL.  Compose provides all the capability of Docker Compose and is intended to be the long term replacement, avoiding the need for the below installation as most `docker-compose` commands are included.

However, it is to be noted that `Compose` has some differences detailed in this [document](https://docs.docker.com/compose/cli-command-compatibility/)

It is also to be noted that `Compose` will not work with any volume mount ending with a trailing `/`.

For example, this will work:

```
- volumes:
  - ./app:/home/node/app
```

This will not:
```
- volumes:
  - ./app/:/home/node/app/
```

## Installation
Follow the [installation guide](https://docs.docker.com/compose/install/) provided by Docker for your OS.
