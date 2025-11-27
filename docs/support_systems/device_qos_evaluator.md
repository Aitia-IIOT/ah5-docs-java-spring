# DeviceQoSEvaluator

This Support System has the purpose of  collecting and calculating Key Performance Indicators (KPIs) from the devices operating within a Local Cloud. Based on the calculated KPIs this system can assist in finding the best service provider system(s) to fulfill specific requirements.

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_2_0/DeviceQoSEvaluator_sysd.pdf)

## Services

### qualityEvaluation

TODO

Service metadata: <br />

```
{
    "evaluationType": "basic-device-kpi"
}
```

-----

### deviceQualityDataManagement

TODO

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

## Measurement Routines

### Basic

Round-Trip Time (RTT) measurements are performed towards the Local Cloud’s devices without requiring any additional effort. For detailed information about the RTT measurement process, refer to the [SysD](../assets/sysd/5_2_0/DeviceQoSEvaluator_sysd.pdf) document. In order to calculating proper RTT based KPIs, a reasonable `rtt.measurement.timeout` shall be configured. 

This timeout defines the maximum allowed duration for a connection attempt and is treated as the upper bound of the measurement window, meaning that all RTT measurements are normalized relative to this value. Also, this timeout must be always less than the `augmented.measurement.job.interval`.

### Augmented

Augmented measurement groups and types are uniquely identified by hierarchical, [OID-like unique identifiers](./device_qos_evaluator/augmented_measurement_oid.md).

Performing augmented measurements requires two thing:

- Installing a device agent on the hosting devices (hardwares, virtual machines or contaniers).
- Specifying the supported measurements on system level by using system metadata.

**Device Agent**

The device-side agent is a lightweight monitoring server implemented in Python. It continuously samples the current performance in every second per [OID sub-groups](./device_qos_evaluator/augmented_measurement_oid.md#sub-group) and caches the collected values in a 30-second time window. The DeviceQoSEvaluator system periodically retrieves these samples and computes the corresponding [OID statistical](./device_qos_evaluator/augmented_measurement_oid.md#statistics) values, which are then stored for a configurable retention period. Operators can enable or disable specific OID sub-groups through the agent’s configuration.

**System Metadata**

If a system of the Local Cloud is being hosted on a device that running the device agent, it shall advertiese its enabled [OID sub-groups](./device_qos_evaluator/augmented_measurement_oid.md#sub-group) through the ServiceRegistry Core System by using the system metadata the following way:

```
{
    "qos": {
        "deviceAugmented": [ "1.4", "2.1", "3.1", "3.2" ]
    }
}
```

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

:fontawesome-solid-wrench: **max.page.size**

Specifies the maximum number of records a page can contain in case of pageable service responses.

TODO

### Logging configuration

The logging configuration properties can be found in the `log4j2.xml` file located at `src/main/resources`
folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but it is also possible to
override it by an external file. For that use the following command when starting the system:
```
java -jar arrowhead-device-qos-evaluator-x.x.x.jar
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