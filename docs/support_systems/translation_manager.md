# Translation Manager

This Support system can discover whether a consumer can consume possible providers' service operation even if there is no common interface between them or are using different data models. If an appropriate provider is selected, the Support System can build a translation bridge between the consumer and the provider using an independent interface translator and optionally one or two data model translators. The Translation Manager is also storing information about the translation bridges.

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_1_0/translationmanager_sysd.pdf) TODO

## Services

### translationBridge

The purpose of translationBridge is to find providers whose service operation is accessible via a translation bridge using the currently available translators and to build/abort such bridges. This service is offered for Application systems (consumers). 

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_1_0/translation-bridge_sd.pdf) TODO<br />
:material-api: [generic_http (IDD)](../api/translationmanager/translation-bridge-generic-http.md) TODO | [generic_https (IDD)](../api/translationmanager/translation-bridge-generic-http.md) TODO<br />
:material-api: [generic_mqtt (IDD)](../api/translationmanager/translation-bridge-generic-mqtt.md) TODO | [generic_mqtts (IDD)](../api/translationmanager/translation-bridge-generic-mqtt.md) TODO<br />
:material-tag: since: v5.1.0

**discovery**

This service operation checks a list of possible providers for a service operation and returns only those that can be used by the requester via a translation bridge using the currently available translators. 

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-generic-http.md#discovery) TODO | [generic_https]((../api/translationmanager/translation-bridge-generic-http.md#discovery) TODO <br />
:material-arrow-right-thin: Example: [generic_mqtt]((../api/translationmanager/translation-bridge-generic-mqtt.md#discovery) TODO | [generic_mqtts]((../api/translationmanager/translation-bridge-generic-mqtt.md#discovery) TODO

**negotiation**

This service operation allows consumers to create a translation bridge to one selected provider's one particular service operation. It ia possible to do discovery during the negotiation if no prior discovery is happened.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-generic-http.md#negotiation) TODO | [generic_https](../api/translationmanager/translation-bridge-generic-http.md#negotiation) TODO<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-generic-mqtt.md#negotiation) TODO | [generic_mqtts](../api/translationmanager/translation-bridge-generic-mqtt.md#negotiation) TODO

**abort**

This service operation aborts an existing bridge created by the requester. After abortion the targeted service operation will not be available via the bridge.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-generic-http.md#abort) TODO | [generic_https](../api/translationmanager/translation-bridge-generic-http.md#abort) TODO<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-generic-mqtt.md#abort) TODO | [generic_mqtts](../api/translationmanager/translation-bridge-generic-mqtt.md#abort) TODO

-----

### translationReport

The purpose of translationReport service is to provide a tool for the interface translators to send events about their related translation bridges.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_1_0/translation-report_sd.pdf) TODO<br />
:material-api: [generic_http (IDD)](../api/translationmanager/translation-report-generic-http.md) TODO | [generic_https (IDD)](../api/translationmanager/translation-report-generic-http.md) TODO<br />
:material-api: [generic_mqtt (IDD)](../api/translationmanager/translation-report-generic-mqtt.md) TODO | [generic_mqtts (IDD)](../api/translationmanager/translation-report-generic-mqtt.md) TODO<br />
:material-tag: since: v5.1.0

**report**

This service operation reports about an event related to a translation bridge. Events can be about usage, closing or some kind of error.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-report-generic-http.md#report) TODO | [generic_https](../api/translationmanager/translation-report-generic-http.md#report) TODO<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-report-generic-mqtt.md#report) TODO | [generic_mqtts](../api/translationmanager/translation-report-generic-mqtt.md#report) TODO

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