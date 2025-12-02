# Docker - Dispersed Compose

Compose every  [Core](../../../help/definitions.md#core-system)/[Support](../../../help/definitions.md#support-system) system and every database independently. Docker containers, so the [Local Cloud](../../../help/definitions.md#local-cloud) building blocks can be distributed between host machines as desired.

## Requirements

* Make sure you are familiar with the [basics](../../../help/definitions.md).
* Make sure you have [docker installed](https://docs.docker.com/get-started/get-docker/).

## How to do it?

### Database Container

1) Create a folder in your host to store your database setup. (Core/Support system names with '-db' suffix are recommended).

2) Create a `.env` file under your database setup folder and include the following constants:

```
DB_ROOT_PSW=<define-root-database-password>
DB_AH_OPERATOR_PSW=<define-ah-operator-database-password>
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

3) Create a `compose.yaml` file under your system setup folder and copy the selected system's [compose file](#system-compose-files) content into it.
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
        image: aitiaiiot/arrowhead-serviceregistry-db:5.2.0
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
        image: aitiaiiot/arrowhead-serviceorchestration-dynamic-db:5.2.0
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

### SimpleStoreServiceOrchestration-DB

```
version: "3.9"

services:

    serviceorchestration-simple-db:
        image: aitiaiiot/arrowhead-serviceorchestration-simple-db:5.2.0
        container_name: arrowhead-serviceorchestration-simple-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: ${DB_ROOT_PSW}
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: ${DB_AH_OPERATOR_PSW}
        volumes:
            - arrowhead_serviceorchestration_simple_db_volume:/var/lib/mysql
        ports:
            - "7456:3306"

volumes:
    arrowhead_serviceorchestration_simple_db_volume:
```

### ConsumerAuthorization-DB

```
version: "3.9"

services:

    consumerauthorization-db:
        image: aitiaiiot/arrowhead-consumerauthorization-db:5.2.0
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
        image: aitiaiiot/arrowhead-authentication-db:5.2.0
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
        image: aitiaiiot/arrowhead-blacklist-db:5.2.0
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

### TranslationManager-DB

```
version: "3.9"

services:

    translation-manager-db:
        image: aitiaiiot/arrowhead-translation-manager-db:5.2.0
        container_name: arrowhead-translation-manager-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: ${DB_ROOT_PSW}
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: ${DB_AH_OPERATOR_PSW}
        volumes:
            - arrowhead_translation_manager_db_volume:/var/lib/mysql
        ports:
            - "7465:3306"

volumes:
    arrowhead_translation_manager_db_volume:
```

### DeviceQoSEvaluator-DB

```
version: "3.9"

services:

    device-qos-evaluator-db:
        image: aitiaiiot/arrowhead-device-qos-evaluator-db:5.2.0
        container_name: arrowhead-device-qos-evaluator-db
        restart: unless-stopped
        environment:
            MYSQL_ROOT_PASSWORD: ${DB_ROOT_PSW}
            MYSQL_USER: ah-operator
            MYSQL_PASSWORD: ${DB_AH_OPERATOR_PSW}
        volumes:
            - arrowhead_device_qos_evaluator_db_volume:/var/lib/mysql
        ports:
            - "7472:3306"

volumes:
    arrowhead_device_qos_evaluator_db_volume:
```

## System Compose Files

### ServiceRegistry

```
version: "3.9"

services:

    serviceregistry:
        image: aitiaiiot/arrowhead-serviceregistry:5.2.0
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
        image: aitiaiiot/arrowhead-serviceorchestration-dynamic:5.2.0
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

### SimpleStoreServiceOrchestration

```
version: "3.9"

services:

    serviceorchestration-simple:
        image: aitiaiiot/arrowhead-serviceorchestration-simple:5.2.0
        container_name: arrowhead-serviceorchestration-simple
        restart: unless-stopped
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://${DB_ADDRESS}:${DB_PORT}/ah_serviceorchestration_simple?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: ${SERVICE_REGISTRY_ADDRESS}
            SERVICE_REGISTRY_PORT: ${SERVICE_REGISTRY_PORT}
        volumes:
            - ${SYSTEM_SETUP_DIR}/config:/app/config
        ports:
            - "8456:8456"
```

### ConsumerAuthorization

```
version: "3.9"

services:

    consumerauthorization:
        image: aitiaiiot/arrowhead-consumerauthorization:5.2.0
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
        image: aitiaiiot/arrowhead-authentication:5.2.0
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
        image: aitiaiiot/arrowhead-blacklist:5.2.0
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

### TranslationManager

```
version: "3.9"

services:

    translation-manager:
        image: aitiaiiot/arrowhead-translation-manager:5.2.0
        container_name: arrowhead-translation-manager
        restart: unless-stopped
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://${DB_ADDRESS}:${DB_PORT}/ah_translation_manager?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: ${SERVICE_REGISTRY_ADDRESS}
            SERVICE_REGISTRY_PORT: ${SERVICE_REGISTRY_PORT}
        volumes:
            - ${SYSTEM_SETUP_DIR}/config:/app/config
        ports:
            - "8465:8465"
```

### DeviceQoSEvaluator

```
version: "3.9"

services:

    device-qos-evaluator:
        image: aitiaiiot/arrowhead-device-qos-evaluator:5.2.0
        container_name: arrowhead-device-qos-evaluator
        restart: unless-stopped
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://${DB_ADDRESS}:${DB_PORT}/ah_device_qos_evaluator?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: ${SERVICE_REGISTRY_ADDRESS}
            SERVICE_REGISTRY_PORT: ${SERVICE_REGISTRY_PORT}
        volumes:
            - ${SYSTEM_SETUP_DIR}/config:/app/config
        ports:
            - "8472:8472"
```