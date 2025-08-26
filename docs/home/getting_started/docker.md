# Docker

## Overview

Necesarry docker images to easily deploy an Arrowhead Local Cloud are aviable in Docker Hub. What you need is to create your own [docker-compose](https://docs.docker.com/compose/) setup that fulfills your requirements the most.

There are multiple proper approches that are requiring different docker compose solutions. Most common approaches:

* [Aggregated](#aggregated-compose): Compose all systems and their databases. Everything is deployed in the same host machine.
* [Coupled](#coupled-compose): Compose a system with its database. Framework components can be sperated to different host machines.
* [Dispersed](#dispersed-compose): Compose every system and every database independently. Containers can be distributed as desired. 

## Aggregated Compose

### Prepare

TODO

### Aggregated Compose Files

TODO

## Coupled Compose

### Prepare

1) Create a folder to store your setup and volumes. In this example let's have an `arrowhead` folder under the `opt` in a Linux system.

2) Create a folder under the `arrowhead` folder for each arrowhead system you reqiure to run on this host.

3) Create the [coupled compose files](#coupled-compose-files) under the system folder.

4) Run `docker copmose up` command in every system folder (always start with ServiceRegistry).

5) Docker volumes will be created automatically and you already have a default cloud.

```
opt/
└── arrowhead
    └── ServiceRegistry/
        └── docker-compose.yaml
        └── config
            └── application.properties
            └── certificates
                └── ServiceRegistry.p12
                └── truststore.p12
    └── DynamicServiceOrchestration/
        └── docker-compose.yaml
        └── config
            └── application.properties
            └── certificates
                └── DynamicServiceOrchestration.p12
                └── truststore.p12
    └── ConsumerAuthorization/
        └── docker-compose.yaml
        └── config
            └── application.properties
            └── certificates
                └── ConsumerAuthorization.p12
                └── truststore.p12
```

6) Change the configurations if required and restart with `docker compose restart`.

### Coupled Compose Files

#### ServiceRegistry

```
version: "3.9"

services:
    serviceregistry:
        image: aitiaiiot/arrowhead-serviceregistry:5.0.0
        container_name: arrowhead-serviceregistry
        restart: unless-stopped
    depends_on:
        serviceregistry-db:
            condition: service_healthy
    environment:
        - SPRING_DATASOURCE_URL=jdbc:mysql://serviceregistry-db:3306/ah_serviceregistry?serverTimezone=UTC
    volumes:
        - opt/arrowhead/ServiceRegistry/config/application.properties:/app/application.properties
        - opt/arrowhead/ServiceRegistry/config/certificates:/app/certificates
    ports:
        - "8443:8443"

    serviceregistry-db:
        image: aitiaiiot/arrowhead-serviceregistry:5.0.0
        container_name: arrowhead-serviceregistry-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: 3t3f9rdlvr9agkm7
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: cjbmhi8g49q9onwg
        volumes:
            - /opt/arrowhead/ServiceRegistry/data_volume:/var/lib/mysql
        ports:
            - "7443:3306"
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-p3t3f9rdlvr9agkm7"]
            interval: 10s
            timeout: 5s
            retries: 5

volumes:
    arrowhead_service_registry_db_data:
```
**Notes:**

* `application.properties` (and certificates folder) will be copied to the host at first time.
* DB schema will be created on first startup when `/var/lib/mysql` in the docker container is empty.
* `SPRING_DATASOURCE_URL` environment variable overides the `spring.datasource.url` in the `application.properties` and the system is accessing the database through an internal docker network.
* The database can be accessed by the human operators via the exposed port and with the given credentials (`MYSQL_USER`, `MYSQL_PASSWORD`) if necesarry. 
* `SERVICE_REGISTRY_ADDRESS` environment variable overides the `service.registry.address` in the `application.properties`.
* `SERVICE_REGISTRY_PORT` environment variable overides the `service.registry.port` in the `application.properties`.
* Do not use `localhost` or `127.0.0.1` to address the Arrowhead Core or Support systems! Use always the real IP address of the given host machine. Or alternatively you can use the `172.17.0.1` docker bridge gateway address, but only within the same host machine that runs the docker engine.

## Dispersed Compose

### Prepare

TODO

### Aggregated Compose Files

TODO

## Changing Credentials

for production

