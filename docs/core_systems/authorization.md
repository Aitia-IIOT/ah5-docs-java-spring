# System Authentication

This core system provides, manages and validates system identities within an Eclipse Arrowhead Local Cloud (LC).

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_0_0/authentication_sysd.pdf)

## Services

### identity

The purpose of this service is to give, verify and invalidates a proof of identity token. Furthermore, it also allows a system to change its own credentials. The service is offered for both application and core/support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/identity_sd.pdf) <br />
:material-api: [generic-http (IDD)](../api/authentication/identity-generic-http.md) | [generic-https (IDD)](../api/authentication/identity-generic-http.md) <br />
:material-api: [generic-mqtt (IDD)](todo) | [generic-mqtts (IDD)](todo) <br />
:material-tag: since: v5.0.0 

**login**

This service operation acquires a proof of identity token.

:material-arrow-right-thin: Example: [generic-http](../api/authentication/identity-generic-http.md#login) | [generic-https](../api/authentication/identity-generic-http.md#login)<br />
:material-arrow-right-thin: Example: [generic-mqtt](todo) | [generic-mqtts](todo)

**logout**

This service operation invalidates a proof of identity token.

:material-arrow-right-thin: Example: [generic-http](../api/authentication/identity-generic-http.md#logout) | [generic-https](../api/authentication/identity-generic-http.md#logout)<br />
:material-arrow-right-thin: Example: [generic-mqtt](todo) | [generic-mqtts](todo)

**change**

This service operation changes the requester system's own credentials.

:material-arrow-right-thin: Example: [generic-http](../api/authentication/identity-generic-http.md#change) | [generic-https](../api/authentication/identity-generic-http.md#change)<br />
:material-arrow-right-thin: Example: [generic-mqtt](todo) | [generic-mqtts](todo)

**verify**

This service operation checks the validity of a provided token and acquires information about the verified system.

:material-arrow-right-thin: Example: [generic-http](../api/authentication/identity-generic-http.md#verify) | [generic-https](../api/authentication/identity-generic-http.md#verify)<br />
:material-arrow-right-thin: Example: [generic-mqtt](todo) | [generic-mqtts](todo)

### general-management

Its purpose is to get some information about the hosting system’s behavior, such as log entries and configuration settings. The service is offered for administrative support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/general-management_sd.pdf) <br />
:material-api: [generic-http (IDD)](todo) | [generic-https (IDD)](todo) <br />
:material-api: [generic-mqtt (IDD)](todo) | [generic-mqtts (IDD)](todo) <br />
:material-tag: since: v5.0.0 

TODO: cont from here

-----

### identity-management

Its purpose is to manage identities and active sessions in bulk. The different operations provide querying, creating, updating and removing functionalities. The service is offered for administrative support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/identity-management_sd.pdf) <br />
:material-api: [generic-http (IDD)](../api/authentication/identity-management-generic-http.md) | [generic-https (IDD)](../api/authentication/identity-management-generic-http.md) <br />
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
