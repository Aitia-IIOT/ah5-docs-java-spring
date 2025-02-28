# Service Registry

This core system provides the data storage functionality for the information related to the currently and actively offered services within the Local Cloud. It also stores information about the systems that offer and/or can use the previously mentioned services, and optionally data about the devices on which those systems are running.

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_0_0/serviceregistry_sysd.pdf)

## Services

### service-discovery

The purpose of this service is to lookup, register and revoke provided services. The service is offered for both application and core/support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/service-discovery_sd.pdf) <br />
:material-api: [generic-http (IDD)](todo) | [generic-https (IDD)](todo) <br />
:material-api: [generic-mqtt (IDD)](todo) | [generic-mqtts (IDD)](todo) <br />
:material-tag: since: v5.0.0 

**register**

This service operation adds new service instance to the Local Cloud.

:material-arrow-right-thin: Example: [generic-http](todo) | [generic-https](todo)<br />
:material-arrow-right-thin: Example: [generic-mqtt](todo) | [generic-mqtts](todo)

**revoke**

This service operation removes a service instance from the Local Cloud.

:material-arrow-right-thin: Example: [generic-http](todo) | [generic-https](todo)<br />
:material-arrow-right-thin: Example: [generic-mqtt](todo) | [generic-mqtts](todo)

**lookup**

This service operation lists the service instances that match the filtering requirements.

:material-arrow-right-thin: Example: [generic-http](todo) | [generic-https](todo)<br />
:material-arrow-right-thin: Example: [generic-mqtt](todo) | [generic-mqtts](todo)

-----

### system-discovery

The purpose of this service is to lookup, register and revoke systems that are part of (or want to be part of) the Local Cloud. The service is offered for both application and core/support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/system-discovery_sd.pdf) <br />
:material-api: [generic-http (IDD)](../api/serviceregistry/system-discovery-generic-http.md) | [generic-https (IDD)](../api/serviceregistry/system-discovery-generic-http.md) <br />
:material-api: [generic-mqtt (IDD)](../api/serviceregistry/system-discovery-generic-mqtt.md) | [generic-mqtts (IDD)](../api/serviceregistry/system-discovery-generic-mqtt.md) <br />
:material-tag: since: v5.0.0 

**register**

This service operation adds new system to the Local Cloud.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/system-discovery-generic-http.md#register) | [generic-https](../api/serviceregistry/system-discovery-generic-http.md#register)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/system-discovery-generic-mqtt.md#register) | [generic-mqtts](../api/serviceregistry/system-discovery-generic-mqtt.md#register)

**revoke**

This service operation removes a system from the Local Cloud.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/system-discovery-generic-http.md#revoke) | [generic-https](../api/serviceregistry/system-discovery-generic-http.md#revoke)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/system-discovery-generic-mqtt.md#revoke) | [generic-mqtts](../api/serviceregistry/system-discovery-generic-mqtt.md#revoke)

**lookup**

This service operation lists the systems that match the filtering requirements.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/system-discovery-generic-http.md#lookup) | [generic-https](../api/serviceregistry/system-discovery-generic-http.md#lookup)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/system-discovery-generic-mqtt.md#lookup) | [generic-mqtts](../api/serviceregistry/system-discovery-generic-mqtt.md#lookup)

-----

### device-discovery

The purpose of this service is to lookup, register and revoke devices on which the Local Cloud’s systems can run. The service is offered for both application and core/support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/device-discovery_sd.pdf) <br />
:material-api: [generic-http (IDD)](../api/serviceregistry/device-discovery-generic-http.md) | [generic-https (IDD)](../api/serviceregistry/device-discovery-generic-http.md) <br />
:material-api: [generic-mqtt (IDD)](../api/serviceregistry/device-discovery-generic-mqtt.md) | [generic-mqtts (IDD)](../api/serviceregistry/device-discovery-generic-mqtt.md) <br />
:material-tag: since: v5.0.0 

**register**

This service operation adds a new device to the Local Cloud.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/device-discovery-generic-http.md/#register) | [generic-https](../api/serviceregistry/device-discovery-generic-http.md/#register)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/device-discovery-generic-mqtt.md/#register) | [generic-mqtts](../api/serviceregistry/device-discovery-generic-mqtt.md/#register)

**revoke**

This service operation removes a device from the Local Cloud.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/device-discovery-generic-http.md/#revoke) | [generic-https](../api/serviceregistry/device-discovery-generic-http.md/#revoke)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/device-discovery-generic-mqtt.md/#revoke) | [generic-mqtts](../api/serviceregistry/device-discovery-generic-mqtt.md/#revoke)

**lookup**

This service operation lists the devices that match the filtering requirements.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/device-discovery-generic-http.md/#lookup) | [generic-https](../api/serviceregistry/device-discovery-generic-http.md/#lookup)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/device-discovery-generic-mqtt.md/#lookup) | [generic-mqtts](../api/serviceregistry/device-discovery-generic-mqtt.md/#lookup)

-----

### service-registry-management

Its purpose is to manage service definitions, service instances, interfaces, systems and devices in bulk. The different operations provide querying, registering, updating and unregistering functionalities. The service is offered for administrative support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/service-registry-management_sd.pdf) <br />
:material-api: [generic-http (IDD)](todo) | [generic-https (IDD)](todo) <br />
:material-api: [generic-mqtt (IDD)](todo) | [generic-mqtts (IDD)](todo) <br />
:material-tag: since: v5.0.0 

TODO

## Configuration

The system configuration properties can be found in the `application.properties` file located at `/src/main/resources` folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but also going to be copied next to the JAR file. Any modification in the configuration file located next to the executable JAR file will override the built in configuration property value.

### General parameters

:fontawesome-solid-wrench: **server.address**

IP address of the server.

:fontawesome-solid-wrench: **server.port**

Port number of the server

### Database parameters

:fontawesome-solid-wrench: **spring.datasource.url**

URL to the database.

:fontawesome-solid-wrench: **spring.datasource.username**

Username to the database.

:fontawesome-solid-wrench: **spring.datasource.password**

Password to the database.

:fontawesome-solid-wrench: **spring.datasource.driver-class-name**

The driver provides the connection to the database and implements the protocol for transferring the query
and result between client and database.

:fontawesome-solid-wrench: **spring.jpa.show-sql**

Set to true in order to log out the SQL queries.

:fontawesome-solid-wrench: **spring.jpa.properties.hibernate.format sql**

Set to true to log out SQL queries in pretty format. (Effective only when 'spring.jpa.show-sql' is 'true')

:fontawesome-solid-wrench: **spring.jpa.hibernate.ddl-auto**

Auto initialization of database tables. Value must be always 'none'.

### Custom parameters

TODO

### Logging configuration

The logging configuration properties can be found in the `log4j2.xml` file located at `src/main/resources`
folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but it is also possible to
override it by an external file. For that use the following command when starting the system:
```
java -jar arrowhead-serviceregistry-x.x.x
     -Dlog4j.configurationFile=path-to-external-file
```

:fontawesome-solid-wrench: **JDBC_LEVEL**

Set this to change the level of log messages in the database. Levels: ALL, TRACE, DEBUG, INFO, WARN,
ERROR, FATAL, OFF.

:fontawesome-solid-wrench: **CONSOLE FILE LEVEL**

Set this to change the level of log messages in consol and the log file. Levels: ALL, TRACE, DEBUG,
INFO, WARN, ERROR, FATAL, OFF.

:fontawesome-solid-wrench: **LOG_DIR**

Set this to change the directory of log files.

## Changelog

#### :material-tag: v5.0.0 

- Initial 5th generation release.
