# Docker - Aggregated Compose

Compose all [Core](../../../help/definitions.md#core-system)/[Support](../../../help/definitions.md#support-system) systems and their databases at once. A complete [Local Cloud](../../../help/definitions.md#local-cloud) can deployed to a single host machine.

## Requirements

* Make sure you are familiar with the [basics](../../../help/definitions.md).
* Make sure you have [docker installed](https://docs.docker.com/get-started/get-docker/).

## How to do it?

1) Creat a folder to store your Local Cloud setup.

2) Create a `.env` file under your cloud folder and include the following constants:

```
CLOUD_DIR=<absolute/path/to/your/cloud/folder>
DOMAIN_NAME=<real-ip-of-your-host-machine>
DB_ROOT_PSW=<define-root-database-password>
DB_AH_OPERATOR_PSW=<define-root-database-password>
```

3) Create a `compose.yaml` file under your cloud folder and copy the compose file contet into for a [Basic](./docker_aggregated.md#basic-arrowhead-cloud) or a [Full](./docker_aggregated.md#full-arrowhead-cloud) Arrowhead Local Cloud. 
>**Learn more** about the [compose file](./about_compose_file.md).

>**Hint:** You might exclude certain Support Systems by removing the associated sections from the Full cloud compose file.

4) Run `docker compose up -d` command in your cloud folder in order to launch your Local Cloud with default config.

   - _Data persistance_: Named volume for the databases will be created automatically into docker's default volume location.
   - _System configuration_: Configuration files will be synced automatically from the container to the host's file system (bind mount) under to `<your-cloud-folder>/<arrowhed-system-folder>/config`. Examlple:

```
MyArrowheadCloud/
└── ServiceRegistry/
    └── config/
        └── certificate/
            └── ServiceRegistry.p12
            └── truststore.p12 
        └── application.properties
        └── log4j2.xml
└── DynamicServiceOrchestration/
    └── config/
        └── certificate/
            └── DynamicServiceOrchestration.p12
            └── truststore.p12 
        └── application.properties
        └── log4j2.xml
└── ConsumerAuthorization/
    └── config/
        └── certificate/
            └── ConsumerAuthorization.p12
            └── truststore.p12 
        └── application.properties
        └── log4j2.xml
└── .env
└── compose.yaml
```

>**Note:** If Authentication Core System is included, your cloud with the default configuration will fail at first time, but config files will be synced to your host. You have to change the configuration to enable the [outsourced](../../../api/authentication_policy.md#outsourced) authentication. See the [authentication.policy](../../../general/general_config_props.md) configuration property.

5) Change the configurations if required

   - Stop the containers with `docker compose stop`.
   - Edit the `application.properties` and/or `log4j2.xml` files and/or change the certificates.
   >  Look for the configuration possibilities in the system descriptions ([example](../../../core_systems/service_registry.md#configuration)).
   - Start the containers with `docker compose start`.

## Compose File

### Basic Arrowhead Cloud

```
version: "3.9"

services:

    # ------------------------------------------
    # ServiceRegistry
    # ------------------------------------------

    serviceregistry-db:
        image: aitiaiiot/arrowhead-serviceregistry-db:5.0.0
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
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    serviceregistry:
        image: aitiaiiot/arrowhead-serviceregistry:5.0.0
        container_name: arrowhead-serviceregistry
        restart: unless-stopped
        depends_on:
            serviceregistry-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://serviceregistry-db:3306/ah_serviceregistry?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
        volumes:
            - ${CLOUD_DIR}/ServiceRegistry/config:/app/config
        ports:
            - "8443:8443"

    # ------------------------------------------    
    # DynamicServiceOrchestration
    # ------------------------------------------

    serviceorchestration-dynamic-db:
        image: aitiaiiot/arrowhead-serviceorchestration-dynamic-db:5.0.0
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
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    serviceorchestration-dynamic:
        image: aitiaiiot/arrowhead-serviceorchestration-dynamic:5.0.0
        container_name: arrowhead-serviceorchestration-dynamic
        restart: unless-stopped
        depends_on:
            serviceorchestration-dynamic-db:
                condition: service_healthy
            serviceregistry:
                condition: service_started
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://serviceorchestration-dynamic-db:3306/ah_serviceorchestration_dynamic?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: serviceregistry
        volumes:
            - ${CLOUD_DIR}/DynamicServiceOrchestration/config:/app/config
        ports:
            - "8441:8441"
            
    # ------------------------------------------
    # ConsumerAuthorization
    # ------------------------------------------
            
    consumerauthorization-db:
        image: aitiaiiot/arrowhead-consumerauthorization-db:5.0.0
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
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    consumerauthorization:
        image: aitiaiiot/arrowhead-consumerauthorization:5.0.0
        container_name: arrowhead-consumerauthorization
        restart: unless-stopped
        depends_on:
            consumerauthorization-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://consumerauthorization-db:3306/ah_consumer_authorization?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: serviceregistry
        volumes:
            - ${CLOUD_DIR}/ConsumerAuthorization/config:/app/config
        ports:
            - "8445:8445"
            
volumes:
    arrowhead_serviceregistry_db_volume:
    arrowhead_serviceorchestration_dynamic_db_volume:
    arrowhead_consumerauthorization_db_volume:
```

### Full Arrowhead Cloud

```
version: "3.9"

services:

    # ------------------------------------------
    # ServiceRegistry Core System
    # ------------------------------------------

    serviceregistry-db:
        image: aitiaiiot/arrowhead-serviceregistry-db:5.0.0
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
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    serviceregistry:
        image: aitiaiiot/arrowhead-serviceregistry:5.0.0
        container_name: arrowhead-serviceregistry
        restart: unless-stopped
        depends_on:
            serviceregistry-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://serviceregistry-db:3306/ah_serviceregistry?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
        volumes:
            - ${CLOUD_DIR}/ServiceRegistry/config:/app/config
        ports:
            - "8443:8443"

    # ------------------------------------------    
    # DynamicServiceOrchestration Core System
    # ------------------------------------------

    serviceorchestration-dynamic-db:
        image: aitiaiiot/arrowhead-serviceorchestration-dynamic-db:5.0.0
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
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    serviceorchestration-dynamic:
        image: aitiaiiot/arrowhead-serviceorchestration-dynamic:5.0.0
        container_name: arrowhead-serviceorchestration-dynamic
        restart: unless-stopped
        depends_on:
            serviceorchestration-dynamic-db:
                condition: service_healthy
            serviceregistry:
                condition: service_started
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://serviceorchestration-dynamic-db:3306/ah_serviceorchestration_dynamic?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: serviceregistry
        volumes:
            - ${CLOUD_DIR}/DynamicServiceOrchestration/config:/app/config
        ports:
            - "8441:8441"
            
    # ------------------------------------------
    # ConsumerAuthorization Core System
    # ------------------------------------------
            
    consumerauthorization-db:
        image: aitiaiiot/arrowhead-consumerauthorization-db:5.0.0
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
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    consumerauthorization:
        image: aitiaiiot/arrowhead-consumerauthorization:5.0.0
        container_name: arrowhead-consumerauthorization
        restart: unless-stopped
        depends_on:
            consumerauthorization-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://consumerauthorization-db:3306/ah_consumer_authorization?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: serviceregistry
        volumes:
            - ${CLOUD_DIR}/ConsumerAuthorization/config:/app/config
        ports:
            - "8445:8445"
            
    # ------------------------------------------
    # Authentication Core System
    # ------------------------------------------
    
    authentication-db:
        image: aitiaiiot/arrowhead-authentication-db:5.0.0
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
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    authentication:
        image: aitiaiiot/arrowhead-authentication:5.0.0
        container_name: arrowhead-authentication
        restart: unless-stopped
        depends_on:
            authentication-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://authentication-db:3306/ah_authentication?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: serviceregistry
        volumes:
            - ${CLOUD_DIR}/Authentication/config:/app/config
        ports:
            - "8444:8444"
        
    # ------------------------------------------
    # Blacklist Support System
    # ------------------------------------------
    
    blacklist-db:
        image: aitiaiiot/arrowhead-blacklist-db:5.0.0
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
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
            interval: 4s
            retries: 5

    blacklist:
        image: aitiaiiot/arrowhead-blacklist:5.0.0
        container_name: arrowhead-blacklist
        restart: unless-stopped
        depends_on:
            blacklist-db:
                condition: service_healthy
        environment:
            SPRING_DATASOURCE_URL: jdbc:mysql://blacklist-db:3306/ah_blacklist?serverTimezone=UTC
            DOMAIN_NAME: ${DOMAIN_NAME}
            SERVICE_REGISTRY_ADDRESS: serviceregistry
        volumes:
            - ${CLOUD_DIR}/Blacklist/config:/app/config
        ports:
            - "8464:8464"
    
volumes:
    arrowhead_serviceregistry_db_volume:
    arrowhead_serviceorchestration_dynamic_db_volume:
    arrowhead_consumerauthorization_db_volume:
    arrowhead_authentication_db_volume:
    arrowhead_blacklist_db_volume:
```