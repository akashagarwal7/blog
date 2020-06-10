---
layout: post
title: Connecting 2 Docker Compose projects on the same network
# subtitle: Each post also has a subtitle
# gh-repo: akashagarwal7/blog
# gh-badge: [star, fork, follow]
tags: [docker, docker-compose]
comments: true
---

Lately, I have been loving using Docker for almost everything. Recently at work, I needed a docker-compose project to join an existing one's network, mainly to access the existing project's MySQL instance. If you're looking for the same, look no further!

## Project name

First, you would want to setup a project name for your docker-compose project, the project whose network you would want other project(s) to join; instead of docker setting up one based on the folder the compose file exists in. This step is optional, but I recommend doing this. To set a project name, all you need to do is:

```sh
docker-compose -p potato_app up
```

This creates a network named `potato_app_default` for the services defined in your compose file. Now, we can configure the compose file of the other project to connect to this network!

## Configuring the compose file for the other project

Let's look at the following docker-compose.yml file:

```yml
version: "3"
services:
  app:
    container_name: my-app
    build: .
    restart: always
    environment:
      MYSQL_DB_ADAPTER: ${MYSQL_DB_ADAPTER}
      MYSQL_DB_DATABASE: ${MYSQL_DB_DATABASE}
      MYSQL_DB_USERNAME: ${MYSQL_DB_USERNAME}
      MYSQL_DB_PASSWORD: ${MYSQL_DB_PASSWORD}
      MYSQL_DB_HOST: ${MYSQL_DB_HOST}
      MYSQL_DB_PORT: ${MYSQL_DB_PORT}
    command: bash -c "node index.js"
    ports:
      - "80:8000"
networks:
  default:
    external:
      name: potato_app_default
```

The main part to look at here is the `networks` key. What the above configuration does is that **it instructs Docker to join an external network** named `potato_app_default` by default, instead of creating its own network. Here onwards, the service app can communicate with other services on the network named `potato_app_default`.

Notice the `ports` section? This maps the internal port `8000` of the container to port `80` of the host, like it normally should!
