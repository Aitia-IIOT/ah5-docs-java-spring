# Translation Manager

This Support system can discover whether a consumer can consume possible providers' service operation even if there is no common interface between them or are using different data models. If an appropriate provider is selected, the Support System can build a translation bridge between the consumer and the provider using an independent interface translator and optionally one or two data model translators. The Translation Manager is also storing information about the translation bridges.

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_1_0/translationmanager_sysd.pdf) 

## Services

### translationBridge

The purpose of translationBridge is to find providers whose service operation is accessible by the requester via a translation bridge using the currently available translators and to build/abort such bridges. This service is offered for Application systems (consumers). 

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_1_0/translation-bridge_sd.pdf)<br />
:material-api: [generic_http (IDD)](../api/translationmanager/translation-bridge-generic-http.md) | [generic_https (IDD)](../api/translationmanager/translation-bridge-generic-http.md)<br />
:material-api: [generic_mqtt (IDD)](../api/translationmanager/translation-bridge-generic-mqtt.md) | [generic_mqtts (IDD)](../api/translationmanager/translation-bridge-generic-mqtt.md)<br />
:material-tag: since: v5.1.0

**discovery**

This service operation checks a list of possible providers for a service operation and returns only those that can be consumed by the requester via a translation bridge using the currently available translators. 

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-generic-http.md#discovery) | [generic_https](../api/translationmanager/translation-bridge-generic-http.md#discovery) <br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-generic-mqtt.md#discovery) | [generic_mqtts](../api/translationmanager/translation-bridge-generic-mqtt.md#discovery) 

**negotiation**

This service operation allows consumers to create a translation bridge to one selected provider's one particular service operation. It is possible to do discovery during the negotiation if no prior discovery is happened.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-generic-http.md#negotiation) | [generic_https](../api/translationmanager/translation-bridge-generic-http.md#negotiation) <br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-generic-mqtt.md#negotiation) | [generic_mqtts](../api/translationmanager/translation-bridge-generic-mqtt.md#negotiation)

**abort**

This service operation aborts an existing bridge created by the requester. After abortion the targeted service operation will not be available via the bridge.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-generic-http.md#abort) | [generic_https](../api/translationmanager/translation-bridge-generic-http.md#abort) <br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-generic-mqtt.md#abort) | [generic_mqtts](../api/translationmanager/translation-bridge-generic-mqtt.md#abort)

-----

### translationReport

The purpose of translationReport service is to provide a tool to send events about translation bridges. This service is offered for interface translator systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_1_0/translation-report_sd.pdf)<br />
:material-api: [generic_http (IDD)](../api/translationmanager/translation-report-generic-http.md) | [generic_https (IDD)](../api/translationmanager/translation-report-generic-http.md)<br />
:material-api: [generic_mqtt (IDD)](../api/translationmanager/translation-report-generic-mqtt.md) | [generic_mqtts (IDD)](../api/translationmanager/translation-report-generic-mqtt.md)<br />
:material-tag: since: v5.1.0

**report**

This service operation reports about an event related to a translation bridge. Events can be about usage, closing or some kind of error.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-report-generic-http.md#report) | [generic_https](../api/translationmanager/translation-report-generic-http.md#report)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-report-generic-mqtt.md#report) | [generic_mqtts](../api/translationmanager/translation-report-generic-mqtt.md#report)

-----

### translationBridgeManagement

The main purpose of translationBridgeManagement is to find providers whose service operation is accessible by a specified consumer via a translation bridge using the currently available translators and to build such bridges. It also offers bulk abortion and query functionalities. This service is offered for Core/Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_1_0/translation-bridge-management_sd.pdf)<br />
:material-api: [generic_http (IDD)](../api/translationmanager/translation-bridge-management-generic-http.md) | [generic_https (IDD)](../api/translationmanager/translation-bridge-management-generic-http.md) <br />
:material-api: [generic_mqtt (IDD)](../api/translationmanager/translation-bridge-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/translationmanager/translation-bridge-management-generic-mqtt.md) <br />
:material-tag: since: v5.1.0

**discovery**

This service operation checks a list of possible providers for a service operation and returns only those that can be consumed by the specified consumer via a translation bridge using the currently available translators. 

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-management-generic-http.md#discovery) | [generic_https](../api/translationmanager/translation-bridge-management-generic-http.md#discovery) <br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-management-generic-mqtt.md#discovery) | [generic_mqtts](../api/translationmanager/translation-bridge-management-generic-mqtt.md#discovery)

**negotiation**

This service operation allows to create a translation bridge for a specified consumer to one specified provider's one particular service operation.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-management-generic-http.md#negotiation) | [generic_https](../api/translationmanager/translation-bridge-management-generic-http.md#negotiation) <br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-management-generic-mqtt.md#negotiation) | [generic_mqtts](../api/translationmanager/translation-bridge-management-generic-mqtt.md#negotiation)

**abort**

This service operation aborts a list of existing bridges. After abortion the targeted service operations will not be available via the bridges.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-management-generic-http.md#abort) | [generic_https](../api/translationmanager/translation-bridge-management-generic-http.md#abort) <br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-management-generic-mqtt.md#abort) | [generic_mqtts](../api/translationmanager/translation-bridge-management-generic-mqtt.md#abort)

**query**

This service operation returns bridge details according to the given filters. It is possible to get information about bridges that no longer exists.

:material-arrow-right-thin: Example: [generic_http](../api/translationmanager/translation-bridge-management-generic-http.md#query) | [generic_https](../api/translationmanager/translation-bridge-management-generic-http.md#query) <br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/translationmanager/translation-bridge-management-generic-mqtt.md#query) | [generic_mqtts](../api/translationmanager/translation-bridge-management-generic-mqtt.md#query)

-----

### generalManagement

Its purpose is to get some information about the hosting system’s behavior, such as log entries and configuration settings. The service is offered for administrative Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/general-management_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/general/general-management-generic-http.md) | [generic_https (IDD)](../api/general/general-management-generic-http.md) <br />
:material-api: [generic_mqtt (IDD)](../api/general/general-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/general/general-management-generic-mqtt.md) <br />
:material-tag: since: v5.1.0 

**get-log**

This service operation lists the log entries of the system that match the filtering requirements.

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

:fontawesome-solid-wrench: **authenticator.credentials**

The credentials that this system will use for performing the login operation when the authentication policy is `outsourced`.

```
authenticator.credentials={\
	'<credential-name>': '<credential-value>' \
}
```

:fontawesome-solid-wrench: **enable.management.filter**

Enable or disable authorization for accessing the management services. Can be `true` of `false`.

:fontawesome-solid-wrench: **management.policy**

Way of authorizing the management service requester systems. Can be:
     
- `sysop-only`, when the authenticated requester system has _system-operator_ role that ensures overall management permission.
- `whitelist`, _sysop-only_ and when the authenticated requester system is whitelisted in the _management.whitelist_ configuration property that ensures overall management permission.
- `authorization`, _sysop-only_ and _whitelist_ and when the authenticated requester system has appropriate service permission according to the ConsumerAuthorization Core system.

:fontawesome-solid-wrench: **management.whitelist**

Name of the systems which can access to management services in case of `whitelist` policy is effective.

:fontawesome-solid-wrench: **enable.blacklist.filter**

Enable/disable automatic service requester system name verification against to cloud level blacklist. Can be `true` or `false`.

:fontawesome-solid-wrench: **force.blacklist.filter**

Whether or not the service requests should be refused when the blacklist server is not responding. Can be `true` or `false`.

:fontawesome-solid-wrench: **blacklist.check.exclude.list**

Comma-separated list that contains systems whose requests are served without checking the cloud level blacklist, even if blacklist is enabled.

:fontawesome-solid-wrench: **enable.authorization**

Enable or disable the authorization cross-checking during the discovery phase. (when ConsumerAuthorization Arrowhead Core System is deployed to the Local Cloud).

:fontawesome-solid-wrench: **enable.custom.configuration**

Enable or disable the feature to load custom configurations for the translators (when Configuration Arrowhead Support System is deployed to the Local Cloud).

:fontawesome-solid-wrench: **allow.discovery.flags**

Enable or disable the feature to fine-tune the checking behavior of the translationBridgeManagement/discovery operation by the requester (blacklist and authorization checks).

:fontawesome-solid-wrench: **translator.service.min.availability**

Specifies what is the minimum availability time of the interface- and data model translator services in minutes. If the value is non-positive, then service availability won't be checked.

:fontawesome-solid-wrench: **max.page.size**

Specifies the maximum number of records a page can contain in case of pageable service responses.

:fontawesome-solid-wrench: **translation.discovery.max.age**

Specifies after how many hours the translation discovery records can be deleted.

:fontawesome-solid-wrench: **cleaner.job.interval**

Specifies how often (in miliseconds) to check the database for obsoleted discovery records.

### Logging configuration

The logging configuration properties can be found in the `log4j2.xml` file located at `src/main/resources`
folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but it is also possible to
override it by an external file. For that use the following command when starting the system:
```
java -jar arrowhead-translation-manager-x.x.x.jar
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