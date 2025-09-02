# Docker - Coupled Compose

Compose a [Core](../../../help/definitions.md#core-system)/[Support](../../../help/definitions.md#support-system) system with its database. [Local Cloud](../../../help/definitions.md#local-cloud) components can be sperated to different host machines.

## Requirements

* Make sure you are familiar with the [basics](../../../help/definitions.md).
* Make sure you have [docker installed](https://docs.docker.com/get-started/get-docker/).

## How to do it?

In this scenario we will deploy one Core System container with its database container.

1) Create a folder to store your setup. In this example let's have an `ah-serviceregistry` folder under the `opt` in a Linux system (the method is same for every OS and every Arrowhead Core/Support system).

2) Create the [coupled compose files](#compose-files) under the system folder.

```
opt/
└── ah-serviceregistry/
    └── compose.yaml
```
3) Complete the compose file with your variables! Only the ones marked as `<your-variable-goes-here>`.

4) Run `docker compose up -d` command in the `ah-serviceregistry` folder in order to start the component with default config.

   - _Data persistance_: Named volume for the database will be created automatically into docker's default volume location.
   - _System configuration_: Configuration files will be synced from the container to the host's file system (bind mount).
```
opt/
└── ah-serviceregistry/
    └── config/
        └── certificate/
            └── ServiceRegistry.p12
            └── truststore.p12 
        └── application.properties
        └── log4j2.xml
    └── compose.yaml
```

5) Change the configurations if required

   - Stop the containers with `docker compose stop`.
   - Edit the `application.properties` and/or `log4j2.xml` and/or change the certificates.
   >  Look for the configuration possibilities in the system descriptions ([example](../../../core_systems/service_registry.md#configuration)).
   - Start the containers with `docker compose start`.

## Compose Files

**IMPORTANT:**

* `application.properties`, `log4j2.xml` and certificate folder will be copied to the host at first time.
* DB schema will be created on first startup when `/var/lib/mysql` in the docker container is empty.
* `SPRING_DATASOURCE_URL` environment variable overides the `spring.datasource.url` in the `application.properties` and the system is accessing the database through an internal docker network.
* The database can be accessed by the human operators via the exposed port and with the given credentials (`MYSQL_USER`, `MYSQL_PASSWORD`) if necesarry.
* `DOMAIN_NAME` environment variable overides the `domain.name` in the `application.properties`. 
* `SERVICE_REGISTRY_ADDRESS` environment variable overides the `service.registry.address` in the `application.properties`.
* `SERVICE_REGISTRY_PORT` environment variable overides the `service.registry.port` in the `application.properties`.
* Do not use `localhost` or `127.0.0.1` to address the Arrowhead Core or Support systems! Use always the real IP address of the given host machine. Or alternatively you can use the `172.17.0.1` docker bridge gateway address, but only within the same host machine that runs the docker engine.
* To change any environment variable requires to recreate the related container.
    * Recreating a system container always overrides the bind mounts, so the `/config` folder content will be set back to the defaults. Always make sure to backup your configuration before system container recreation.
    * Recreating a database container does not effect the named volumes, so the new container will use the same persistent database files.  

### ServiceRegistry

```
version: "3.9"

services:
    
    serviceregistry-db:
        image: arrowhead-serviceregistry-db:5.0.0
        container_name: arrowhead-serviceregistry-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: <define-a-root-password>
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: <define-an-operator-password>
        volumes:
            - arrowhead_serviceregistry_db_volume:/var/lib/mysql
        ports:
            - "7443:3306"
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    serviceregistry:
        image: arrowhead-serviceregistry:5.0.0
        container_name: arrowhead-serviceregistry
        restart: unless-stopped
        depends_on:
            serviceregistry-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://serviceregistry-db:3306/ah_serviceregistry?serverTimezone=UTC
            DOMAIN_NAME: <real-ip-of-the-host>
        volumes:
            - <path/to/your/system/folder>/config:/app/config
        ports:
            - "8443:8443"
            
volumes:
    arrowhead_serviceregistry_db_volume:
```

### DynamicServiceOrchestration

```
version: "3.9"

services:
    
    serviceorchestration-dynamic-db:
        image: arrowhead-serviceorchestration-dynamic-db:5.0.0
        container_name: arrowhead-serviceorchestration-dynamic-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: <define-a-root-password>
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: <define-an-operator-password>
        volumes:
            - arrowhead_serviceorchestration_dynamic_db_volume:/var/lib/mysql
        ports:
            - "7441:3306"
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    serviceorchestration-dynamic:
        image: arrowhead-serviceorchestration-dynamic:5.0.0
        container_name: arrowhead-serviceorchestration-dynamic
        restart: unless-stopped
        depends_on:
            serviceorchestration-dynamic-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://serviceorchestration-dynamic-db:3306/ah_serviceorchestration_dynamic?serverTimezone=UTC
            DOMAIN_NAME: <real-ip-of-the-host>
            SERVICE_REGISTRY_ADDRESS: <real-ip-of-the-serviceregistry-host>
            SERVICE_REGISTRY_PORT: <exposed-port-of-the-serviceregistry-to-its-host>
        volumes:
            - <path/to/your/system/folder>/config:/app/config
        ports:
            - "8441:8441"
            
volumes:
    arrowhead_serviceorchestration_dynamic_db_volume:
```

### ConsumerAuthorization

```
version: "3.9"

services:
    
    consumerauthorization-db:
        image: arrowhead-consumerauthorization-db:5.0.0
        container_name: arrowhead-consumerauthorization-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: <define-a-root-password>
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: <define-an-operator-password>
        volumes:
            - arrowhead_consumerauthorization_db_volume:/var/lib/mysql
        ports:
            - "7445:3306"
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    consumerauthorization:
        image: arrowhead-consumerauthorization:5.0.0
        container_name: arrowhead-consumerauthorization
        restart: unless-stopped
        depends_on:
            consumerauthorization-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://consumerauthorization-db:3306/ah_consumer_authorization?serverTimezone=UTC
            DOMAIN_NAME: <real-ip-of-the-host>
            SERVICE_REGISTRY_ADDRESS: <real-ip-of-the-serviceregistry-host>
            SERVICE_REGISTRY_PORT: <exposed-port-of-the-serviceregistry-to-its-host>
        volumes:
            - <path/to/your/system/folder>/config:/app/config
        ports:
            - "8445:8445"
            
volumes:
    arrowhead_consumerauthorization_db_volume:
```

### Authentication

```
version: "3.9"

services:
    
    authentication-db:
        image: arrowhead-authentication-db:5.0.0
        container_name: arrowhead-authentication-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: <define-a-root-password>
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: <define-an-operator-password>
        volumes:
            - arrowhead_authentication_db_volume:/var/lib/mysql
        ports:
            - "7444:3306"
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    authentication:
        image: arrowhead-authentication:5.0.0
        container_name: arrowhead-authentication
        restart: unless-stopped
        depends_on:
            authentication-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://authentication-db:3306/ah_authentication?serverTimezone=UTC
            DOMAIN_NAME: <real-ip-of-the-host>
            SERVICE_REGISTRY_ADDRESS: <real-ip-of-the-serviceregistry-host>
            SERVICE_REGISTRY_PORT: <exposed-port-of-the-serviceregistry-to-its-host>
        volumes:
            - <path/to/your/system/folder>/config:/app/config
        ports:
            - "8444:8444"
            
volumes:
    arrowhead_authentication_db_volume:
```

### Blacklist

```
version: "3.9"

services:
    
    blacklist-db:
        image: arrowhead-blacklist-db:5.0.0
        container_name: arrowhead-blacklist-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: <define-a-root-password>
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: <define-an-operator-password>
        volumes:
            - arrowhead_blacklist_db_volume:/var/lib/mysql
        ports:
            - "7464:3306"
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    blacklist:
        image: arrowhead-blacklist:5.0.0
        container_name: arrowhead-blacklist
        restart: unless-stopped
        depends_on:
            blacklist-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://blacklist-db:3306/ah_blacklist?serverTimezone=UTC
            DOMAIN_NAME: <real-ip-of-the-host>
            SERVICE_REGISTRY_ADDRESS: <real-ip-of-the-serviceregistry-host>
            SERVICE_REGISTRY_PORT: <exposed-port-of-the-serviceregistry-to-its-host>
        volumes:
            - <path/to/your/system/folder>/config:/app/config
        ports:
            - "8464:8464"
            
volumes:
    arrowhead_blacklist_db_volume:
```