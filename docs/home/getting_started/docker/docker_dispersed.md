# Docker - Dispersed Compose

Compose every  [Core](../../../help/definitions.md#core-system)/[Support](../../../help/definitions.md#support-system) system and every database independently. Docker containers, so the [Local Cloud](../../../help/definitions.md#local-cloud) building blocks can be distributed between host machines as desired.

## Requirements

* Make sure you are familiar with the [basics](../../../help/definitions.md).
* Make sure you have [docker installed](https://docs.docker.com/get-started/get-docker/).

## How to do it?

### Database Container

1) Creat a folder in your host to store your database setup. (Core/Support system names with '-db' suffix are recommended).

2) Create a `.env` file under your database setup folder and include the following constants:

```
DB_ROOT_PSW=<define-root-database-password>
DB_AH_OPERATOR_PSW=<define-root-database-password>
```

3) Create a `compose.yaml` file under your database setup folder and copy the selected system's [database compose file](#database-compose-files) content into.
>**Learn more** about the [compose file](./about_compose_file.md).

4) Run `docker compose up -d` command in the database setup folder in order to start the database, that will be accessable via the real IP address of your host machine and the exposed port, which is defined in the compose file.

- _Data persistance_: Named volume for the database will be created automatically into docker's default volume location.

### System Container

1) Create a folder in your host to store your system setup. (Core/Support system names are recommended).

2) Create a `.env` file under your system setup folder and include the following constants:

```
SYSTEM_SETUP_DIR=<absolute/path/to/your/system/setup/folder>
DOMAIN_NAME=<real-ip-of-your-host-machine>
DB_ADDRESS=<real-ip-of-the-database-host>
DB_PORT=<exposed-port-of-the-database-to-its-host>

# For every systems that is not ServiceRegistry
SERVICE_REGISTRY_ADDRESS=<real-ip-of-the-serviceregistry-host>
SERVICE_REGISTRY_PORT=<exposed-port-of-the-serviceregistry-to-its-host>
```

3) Create a `compose.yaml` file under your system setup folder and copy the selected sysytem's [compose file](#system-compose-files) contet into.
>**Learn more** about the [compose file](./about_compose_file.md).

4) Run `docker compose up -d` command in the system setup folder in order to start the system with default config.

- _System configuration_: Configuration files will be synced automatically from the container to the host's file system (bind mount). Example:

```
ServiceRegistry/
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

## Database Compose Files

### ServiceRegistry-DB

```
version: "3.9"

services:
    
    serviceregistry-db:
        image: arrowhead-serviceregistry-db:5.0.0
        container_name: arrowhead-serviceregistry-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: ${DB_ROOT_PSW}
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: ${DB_AH_OPERATOR_PSW}
        volumes:
            - arrowhead_serviceregistry_db_volume:/var/lib/mysql
        ports:
            - "7443:3306"
            
volumes:
    arrowhead_serviceregistry_db_volume:
```

### DynamicServiceOrchestration-DB

```
version: "3.9"

services:

    serviceorchestration-dynamic-db:
        image: arrowhead-serviceorchestration-dynamic-db:5.0.0
        container_name: arrowhead-serviceorchestration-dynamic-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: ${DB_ROOT_PSW}
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: ${DB_AH_OPERATOR_PSW}
        volumes:
            - arrowhead_serviceorchestration_dynamic_db_volume:/var/lib/mysql
        ports:
            - "7441:3306"

volumes:
    arrowhead_serviceorchestration_dynamic_db_volume:
```

### ConsumerAuthorization-DB

```
version: "3.9"

services:

    consumerauthorization-db:
        image: arrowhead-consumerauthorization-db:5.0.0
        container_name: arrowhead-consumerauthorization-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: ${DB_ROOT_PSW}
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: ${DB_AH_OPERATOR_PSW}
        volumes:
            - arrowhead_consumerauthorization_db_volume:/var/lib/mysql
        ports:
            - "7445:3306"

volumes:
    arrowhead_consumerauthorization_db_volume:
```

### Authentication-DB

```
version: "3.9"

services:

    authentication-db:
        image: arrowhead-authentication-db:5.0.0
        container_name: arrowhead-authentication-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: ${DB_ROOT_PSW}
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: ${DB_AH_OPERATOR_PSW}
        volumes:
            - arrowhead_authentication_db_volume:/var/lib/mysql
        ports:
            - "7444:3306"

volumes:
    arrowhead_authentication_db_volume:
```

### Blacklist-DB

```
version: "3.9"

services:

    blacklist-db:
        image: arrowhead-blacklist-db:5.0.0
        container_name: arrowhead-blacklist-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: ${DB_ROOT_PSW}
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: ${DB_AH_OPERATOR_PSW}
        volumes:
            - arrowhead_blacklist_db_volume:/var/lib/mysql
        ports:
            - "7464:3306"

volumes:
    arrowhead_blacklist_db_volume:
```

## System Compose Files

### ServiceRegistry

```
version: "3.9"

services:

    serviceregistry:
        image: arrowhead-serviceregistry:5.0.0
        container_name: arrowhead-serviceregistry
        restart: unless-stopped
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://${DB_ADDRESS}:${DB_PORT}/ah_serviceregistry?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
        volumes:
            - ${SYSTEM_SETUP_DIR}/config:/app/config
        ports:
            - "8443:8443"
```

### DynamicServiceOrchestration

```
version: "3.9"

services:

    serviceorchestration-dynamic:
        image: arrowhead-serviceorchestration-dynamic:5.0.0
        container_name: arrowhead-serviceorchestration-dynamic
        restart: unless-stopped
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://${DB_ADDRESS}:${DB_PORT}/ah_serviceorchestration_dynamic?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: ${SERVICE_REGISTRY_ADDRESS}
            SERVICE_REGISTRY_PORT: ${SERVICE_REGISTRY_PORT}
        volumes:
            - ${SYSTEM_SETUP_DIR}/config:/app/config
        ports:
            - "8441:8441"
```

### ConsumerAuthorization

```
version: "3.9"

services:

    consumerauthorization:
        image: arrowhead-consumerauthorization:5.0.0
        container_name: arrowhead-consumerauthorization
        restart: unless-stopped
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://${DB_ADDRESS}:${DB_PORT}/ah_consumer_authorization?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: ${SERVICE_REGISTRY_ADDRESS}
            SERVICE_REGISTRY_PORT: ${SERVICE_REGISTRY_PORT}
        volumes:
            - ${SYSTEM_SETUP_DIR}/config:/app/config
        ports:
            - "8445:8445"
```

### Authentication

```
version: "3.9"

services:

    authentication:
        image: arrowhead-authentication:5.0.0
        container_name: arrowhead-authentication
        restart: unless-stopped
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://${DB_ADDRESS}:${DB_PORT}/ah_authentication?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: ${SERVICE_REGISTRY_ADDRESS}
            SERVICE_REGISTRY_PORT: ${SERVICE_REGISTRY_PORT}
        volumes:
            - ${SYSTEM_SETUP_DIR}/config:/app/config
        ports:
            - "8444:8444"
```

### Blacklist

```
version: "3.9"

services:

    blacklist:
        image: arrowhead-blacklist:5.0.0
        container_name: arrowhead-blacklist
        restart: unless-stopped
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://${DB_ADDRESS}:${DB_PORT}/ah_blacklist?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: ${SERVICE_REGISTRY_ADDRESS}
            SERVICE_REGISTRY_PORT: ${SERVICE_REGISTRY_PORT}
        volumes:
            - ${SYSTEM_SETUP_DIR}/config:/app/config
        ports:
            - "8464:8464"
```