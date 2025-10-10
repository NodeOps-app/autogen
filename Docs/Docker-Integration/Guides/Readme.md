🚧 This page is WIP 🚧

# Adapt a Docker run or Docker compose for AutoGen

This guide demonstrates how to pivot from using a Docker run command or Docker compose file to using AutoGen.

## Prerequisites

- Docker familiarity
- [AutoGen account](https://autogen.nodeops.network/)

## Overview

This guide will walkthrough adapting the Docker commands required to run the public image of a popular blogging platform, [Ghost](https://ghost.org/). 

This image may be started with a backend database or, by applying the correct configuration with a SQLite setting which allows everything to run from one container. The following configurations specify a sqlite3 database to take advantage of this feature.

Review your favorite method:

- [Docker compose](#docker-compose)
- Docker run

and then follow the demonstration on how to create the equivalent on AugoGen.

### Docker compose

<details>
  <summary>Show me the Docker compose file</summary>

```docker-compose.yml
version: '3.8'

services:
  ghost:
    image: ghost:5-alpine
    container_name: ghost-blog
    ports:
      - "2368:2368"
    environment:
      NODE_ENV: production
      database__client: sqlite3
      database__connection__filename: /var/lib/ghost/content/data/ghost.db
      database__useNullAsDefault: "true"
    restart: unless-stopped
```
</details>

### Docker run

<details>
  <summary>Show me the Docker run configurations</summary>

In a situation where I can configure Docker with a dedicated file, I can create a Docker run command that references that, e.g.:

`docker run -d --name ghost-blog -p 2368:2368 \
  -v $(pwd)/config.production.json:/var/lib/ghost/config.production.json \
  -e NODE_ENV=production \
  ghost:5-alpine`

This command:

- Mounts the local config file with: -v $(pwd)/config.production.json:/var/lib/ghost/config.production.json

This allows the configuration file to do some of the heavy lifting. In this config, we can specify that the database should be SQLlite with:

 ```config.production.json
 ... "database": {
    "client": "sqlite3",
    "connection": {
      "filename": "/var/lib/ghost/content/data/ghost.db"
    },
    ...
```

> Click to view the entire file.

  <details>
    <summary>Configuration file for our Docker run</summary>

  ```config.production.json
  {
    "url": "http://localhost:2368",
    "database": {
      "client": "sqlite3",
      "connection": {
        "filename": "/var/lib/ghost/content/data/ghost.db"
      },
      "useNullAsDefault": true
    },
    "server": {
      "host": "0.0.0.0",
      "port": 2368
    },
    "mail": {
      "transport": "Direct"
    },
    "logging": {
      "transports": [
        "file",
        "stdout"
      ],
      "level": "info"
    },
    "process": "systemd",
    "security": {
      "staffDeviceVerification": true
    },
    "paths": {
      "contentPath": "/var/lib/ghost/content"
    }
  }
  ```

  </details>

</details>