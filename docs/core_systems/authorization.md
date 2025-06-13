# ConsumerAuthorization

This Core system manages and to authorizes connections between various systems using authorization rules within an Eclipse Arrowhead Local Cloud (LC).
It also provides various token generation functionalities that adds an extra layer of security.

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_0_0/authorization_sysd.pdf)

## Services

### authorization

The purpose of this service is to validate service consumption permissions and to lookup, grant and revoke those permissions. The service is offered for both application and Core/Support Systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/authorization_sd.pdf) TODO<br />
:material-api: [generic_http (IDD)](../api/consumerauthorization/authorization-generic-http.md) | [generic_https (IDD)](../api/consumerauthorization/authorization-generic-http.md) TODO<br />
:material-api: [generic_mqtt (IDD)](../api/consumerauthorization/authorization-generic-mqtt.md) | [generic_mqtts (IDD)](../api/consumerauthorization/authorization-generic-mqtt.md) TODO<br />
:material-tag: since: v5.0.0 

**grant**

This service operation enables a provider to grant access to various consumers to its service. It can also be used by a publisher to grant access to subscribers to its event with a specific type.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-generic-http.md#grant) | [generic_https](../api/consumerauthorization/authorization-generic-http.md#grant) TODO<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-generic-mqtt.md#grant) | [generic_mqtts](../api/consumerauthorization/authorization-generic-mqtt.md#grant) TODO

**revoke**

This service operation enables a provider/publisher to remove existing authorization policies that was created by itself.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-generic-http.md#revoke) | [generic_https](../api/consumerauthorization/authorization-generic-http.md#revoke) TODO<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-generic-mqtt.md#revoke) | [generic_mqtts](../api/consumerauthorization/authorization-generic-mqtt.md#revoke) TODO

**lookup**

This service operation lists the requester-created authorization rules that match the filtering requirements.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-generic-http.md#lookup) | [generic_https](../api/consumerauthorization/authorization-generic-http.md#lookup) TODO<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-generic-mqtt.md#lookup) | [generic_mqtts](../api/consumerauthorization/authorization-generic-mqtt.md#lookup) TODO

**verify**

This service operation checks whether a consumer has access to a specified service/service operation/event type.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-generic-http.md#verify) | [generic_https](../api/consumerauthorization/authorization-generic-http.md#verify) TODO<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-generic-mqtt.md#verify) | [generic_mqtts](../api/consumerauthorization/authorization-generic-mqtt.md#verify) TODO

### authorizationToken

The purpose of this service is to generate and validate authorization tokens. The service is offered for both application and Core/Support Systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/authorization-token_sd.pdf) TODO<br />
:material-api: [generic_http (IDD)](../api/consumerauthorization/authorization-token-generic-http.md) | [generic_https (IDD)](../api/consumerauthorization/authorization-token-generic-http.md) TODO<br />
:material-api: [generic_mqtt (IDD)](../api/consumerauthorization/authorization-token-generic-mqtt.md) | [generic_mqtts (IDD)](../api/consumerauthorization/authorization-token-generic-mqtt.md) TODO<br />
:material-tag: since: v5.0.0 

TODO fill it with authorizationToken's operations

### generalManagement

Its purpose is to get some information about the hosting system's behavior, such as log entries and configuration settings. The service is offered for administrative Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/general-management_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/general/general-management-generic-http.md) | [generic_https (IDD)](../api/general/general-management-generic-http.md) <br />
:material-api: [generic_mqtt (IDD)](../api/general/general-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/general/general-management-generic-mqtt.md) <br />
:material-tag: since: v5.0.0 

**get-log**

This service operation lists the log entries of the system that matches the filtering requirements.

:material-arrow-right-thin: Example: [generic_http](../api/general/general-management-generic-http.md#get-log) | [generic_https](../api/general/general-management-generic-http.md#get-log)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/general/general-management-generic-mqtt.md#get-log) | [generic_mqtts](../api/general/general-management-generic-mqtt.md#get-log)

**get-config**

This service operation lists the current values of the specified configuration settings.

:material-arrow-right-thin: Example: [generic_http](../api/general/general-management-generic-http.md#get-config) | [generic_https](../api/general/general-management-generic-http.md#get-config)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/general/general-management-generic-mqtt.md#get-config) | [generic_mqtts](../api/general/general-management-generic-mqtt.md#get-config)

### authorizationManagement

The purpose of this service is to manage (query, grant, revoke) authorization rules and validate service consumption permissions in bulk. The service is offered for Core and administrative Support systems.  

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/authorization-management_sd.pdf) TODO<br />
:material-api: [generic_http (IDD)](../api/consumerauthorization/authorization-management-generic-http.md) | [generic_https (IDD)](../api/consumerauthorization/authorization-management-generic-http.md) TODO<br />
:material-api: [generic_mqtt (IDD)](../api/consumerauthorization/authorization-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/consumerauthorization/authorization-management-generic-mqtt.md) TODO<br />
:material-tag: since: v5.0.0 

TODO: continue from here

-----

TODO: change the configuration section

## Configuration

The system configuration properties can be found in the `application.properties` file located at `/src/main/resources` folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but also going to be copied next to the JAR file. Any modification in the configuration file located next to the executable JAR file will override the built in configuration property value.

### General parameters

See the [general configuration properties](../general/general_config_props.md).

**_Note:_** In case of the Authentication system the property **authentication.policy** has a special value `internal`, which means the system should use its own database during authentication. The property should not be changed.

### Database parameters

:fontawesome-solid-wrench: **spring.datasource.url**

Full connection URL to the database.

:fontawesome-solid-wrench: **spring.datasource.username**

Username to the database.

:fontawesome-solid-wrench: **spring.datasource.password**

Password to the database.

:fontawesome-solid-wrench: **spring.datasource.driver-class-name**

The driver provides the connection to the database and implements the protocol for transferring the query
and result between client and database.

:fontawesome-solid-wrench: **spring.jpa.show-sql**

Set to `true` in order to log out the SQL queries.

:fontawesome-solid-wrench: **spring.jpa.properties.hibernate.format sql**

Set to `true` to log out SQL queries in pretty format. (Effective only when 'spring.jpa.show-sql' is 'true')

:fontawesome-solid-wrench: **spring.jpa.hibernate.ddl-auto**

Auto initialization of database tables. Value must be always 'none'.

### Custom parameters

:fontawesome-solid-wrench: **authentication.secret.key**

The secret key which is used to prove to the Local Cloud's ServiceRegistry that this authentication is trusted. This secret key must be present in the Service Registry **authenticator.secret.keys** structure.

:fontawesome-solid-wrench: **enable.management.filter**

Set to `true` to enable automatic authorization for management services.

:fontawesome-solid-wrench: **management.policy**

Defines the access policy for management services. Can be `sysop-only` (only systems with system operator permission can use them), `whitelist` (system operators and those dedicated systems that appear on the **management.whitelist** can use them) or `authorization` (system operators, whitelist members and those systems that have permission according to the ConsumerAuthorization system can use them).

:fontawesome-solid-wrench: **management.whitelist**

A list of system names (separated by comma) that can use management services if the **management.policy** is set to `whitelist` or `authorization`.

:fontawesome-solid-wrench: **identity.token.duration**

Validity period of the identity token in seconds (0 or negative value means hundred years).

:fontawesome-solid-wrench: **cleaner.job.interval**

Interval between execution times of the expired session cleaner job in milliseconds.

### Logging configuration

The logging configuration properties can be found in the `log4j2.xml` file located at `src/main/resources`
folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but it is also possible to
override it by an external file. For that use the following command when starting the system:
```
java -jar arrowhead-consumer-authentication-5.x.x.jar
     -Dlog4j.configurationFile=path-to-external-file
```

:fontawesome-solid-wrench: **JDBC_LEVEL**

Set this to change the level of log messages in the database. Levels: `ALL`, `TRACE`, `DEBUG`, `INFO`, `WARN`,
`ERROR`, `FATAL`, `OFF`.

:fontawesome-solid-wrench: **CONSOLE_FILE_LEVEL**

Set this to change the level of log messages in console and the log file. Levels: `ALL`, `TRACE`, `DEBUG`, `INFO`, `WARN`,
`ERROR`, `FATAL`, `OFF`.

:fontawesome-solid-wrench: **LOG_DIR**

Set this to change the directory of log files.

## Changelog

#### :material-tag: v5.0.0 

Related in CL-5.0.0

- [general](../general/changelogs/cl500.md#general)
- [arrowhead-common-utils](../general/changelogs/cl500.md#arrowhead-common-utils)
- [arrowhead-data-transfer-objects](../general/changelogs/cl500.md#arrowhead-data-transfer-objects)
- [arrowhead-consumer-authorization](../general/changelogs/cl500.md#arrowhead-consumer-authorization)