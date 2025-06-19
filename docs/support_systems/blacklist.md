# Blacklist

This Support system makes it possible for systems with operator role or proper permissions to ban other systems from the Local Cloud. 

There are operations that provide some information about the blacklist. These are available for every system.

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_0_0/blacklist_sysd.pdf)

## Services

### blacklistDiscovery

The purpose of blacklistDiscovery is to provide information about the blacklist. This service is offered for both Application and Core/Support systems. 

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/blacklistDiscovery_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/blacklist/blacklistDiscovery-generic-http.md) | [generic_https (IDD)](../api/blacklist/blacklistDiscovery-generic-http.md) <br />
:material-api: [generic_mqtt (IDD)](../api/blacklist/blacklistDiscovery-generic-mqtt.md) | [generic_mqtts (IDD)](../api/blacklist/blacklistDiscovery-generic-mqtt.md) <br />
:material-tag: since: v5.0.0

**lookup**

This service operation returns the blacklist entries that are in force and apply to the requester. Note that _lookup_ is enabled even if the requester is blacklisted.

:material-arrow-right-thin: Example: [generic_http](../api/blacklist/blacklistDiscovery-generic-http.md#lookup) | [generic_https](../api/blacklist/blacklistDiscovery-generic-http.md#lookup)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/blacklist/blacklistDiscovery-generic-mqtt.md#lookup) | [generic_mqtts](../api/blacklist/blacklistDiscovery-generic-mqtt.md#lookup)

**check**

This service operation allows systems to check whether another system is blacklisted.

:material-arrow-right-thin: Example: [generic_http](../api/blacklist/blacklistDiscovery-generic-http.md#check) | [generic_https](../api/blacklist/blacklistDiscovery-generic-http.md#check)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/blacklist/blacklistDiscovery-generic-mqtt.md#check) | [generic_mqtts](../api/blacklist/blacklistDiscovery-generic-mqtt.md#check)

-----

### blacklistManagement

Its purpose is to manage (query, create and remove) blacklist entries in bulk. The service is offered for administrative Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/blacklistManagement_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/blacklist/blacklistManagement-generic-http.md) | [generic_https (IDD)](../api/blacklist/blacklistManagement-generic-http.md) <br />
:material-api: [generic_mqtt (IDD)](../api/blacklist/blacklistManagement-generic-mqtt.md) | [generic_mqtts (IDD)](../api/blacklist/blacklistManagement-generic-mqtt.md) <br />
:material-tag: since: v5.0.0 

**query**

This service operation returns existing blacklist entries according to the given filters.

:material-arrow-right-thin: Example: [generic_http](../api/blacklist/blacklistManagement-generic-http.md#query) | [generic_https](../api/blacklist/blacklistManagement-generic-http.md#query)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/blacklist/blacklistManagement-generic-mqtt.md#query) | [generic_mqtts](../api/blacklist/blacklistManagement-generic-mqtt.md#query)

**create**

This service operation creates active blacklist entries in bulk.

:material-arrow-right-thin: Example: [generic_http](../api/blacklist/blacklistManagement-generic-http.md#create) | [generic_https](../api/blacklist/blacklistManagement-generic-http.md#create)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/blacklist/blacklistManagement-generic-mqtt.md#create) | [generic_mqtts](../api/blacklist/blacklistManagement-generic-mqtt.md#create)

**remove**

This service operation inactivates every entry that applies to the specified systems. Note that this will not remove the entry from the database.

:material-arrow-right-thin: Example: [generic_http](../api/blacklist/blacklistManagement-generic-http.md#remove) | [generic_https](../api/blacklist/blacklistManagement-generic-http.md#remove)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/blacklist/blacklistManagement-generic-mqtt.md#remove) | [generic_mqtts](../api/blacklist/blacklistManagement-generic-mqtt.md#remove)

-----

### generalManagement

Its purpose is to get some information about the hosting system’s behavior, such as log entries and configuration settings. The service is offered for administrative Support systems.

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

## Configuration

The system configuration properties can be found in the `application.properties` file located at `/src/main/resources` folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but also going to be copied next to the JAR file. Any modification in the configuration file located next to the executable JAR file will override the built in configuration property value.

### General parameters

See the [general configuration properties](../general/general_config_props.md).

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

:fontawesome-solid-wrench: **enable.management.filter**

Enable or disable authorization for accessing the management services. Can be `true` of `false`.

:fontawesome-solid-wrench: **management.policy**

Way of authorizing the management service requester systems. Can be:
     
- `sysop-only`, when the authenticated requester system has _system-operator_ role that ensures overall management permission.
- `whitelist`, _sysop-only_ and when the authenticated requester system is whitelisted in the _management.whitelist_ configuration property that ensures overall management permission.
- `authorization`, _sysop-only_ and _whitelist_ and when the authenticated requester system has appropriate service permission according to the ConsumerAuthorization Core system.

:fontawesome-solid-wrench: **management.whitelist**

Name of the systems which can access to management services in case of `whitelist` policy is effective.

:fontawesome-solid-wrench: **max.page.size**

Specifies the maximum number of records a page can contain in case of pageable service responses.

:fontawesome-solid-wrench: **whitelist**

Name of the systems that cannot be blacklisted. By starting the application, existing blacklist records belonging to these systems will be inactivated as well.

### Logging configuration

The logging configuration properties can be found in the `log4j2.xml` file located at `src/main/resources`
folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but it is also possible to
override it by an external file. For that use the following command when starting the system:
```
java -jar arrowhead-blacklist-x.x.x.jar
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

Related in CL-5.0.0

- [general](../general/changelogs/cl500.md#general)
- [arrowhead-common-utils](../general/changelogs/cl500.md#arrowhead-common-utils)
- [arrowhead-data-transfer-objects](../general/changelogs/cl500.md#arrowhead-data-transfer-objects)
- [arrowhead-blacklist](../general/changelogs/cl500.md#arrowhead-blacklist)