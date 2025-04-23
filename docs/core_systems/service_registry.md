# Service Registry

This core system provides the data storage functionality for the information related to the currently and actively offered services within the Local Cloud. It also stores information about the systems that offer and/or can use the previously mentioned services, and optionally data about the devices on which those systems are running.

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_0_0/serviceregistry_sysd.pdf)

## Services

### service-discovery

The purpose of this service is to lookup, register and revoke provided services. The service is offered for both application and core/support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/service-discovery_sd.pdf) <br />
:material-api: [generic-http (IDD)](../api/serviceregistry/service-discovery-generic-http.md) | [generic-https (IDD)](../api/serviceregistry/service-discovery-generic-http.md) <br />
:material-api: [generic-mqtt (IDD)](../api/serviceregistry/service-discovery-generic-mqtt.md) | [generic-mqtts (IDD)](../api/serviceregistry/service-discovery-generic-mqtt.md) <br />
:material-tag: since: v5.0.0 

**register**

This service operation adds new service instance to the Local Cloud.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/service-discovery-generic-http.md#register) | [generic-https](../api/serviceregistry/service-discovery-generic-http.md#register)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/service-discovery-generic-mqtt.md#register) | [generic-mqtts](../api/serviceregistry/service-discovery-generic-mqtt.md#register)

**revoke**

This service operation removes a service instance from the Local Cloud.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/service-discovery-generic-http.md#revoke) | [generic-https](../api/serviceregistry/service-discovery-generic-http.md#revoke)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/service-discovery-generic-mqtt.md#revoke) | [generic-mqtts](../api/serviceregistry/service-discovery-generic-mqtt.md#revoke)

**lookup**

This service operation lists the service instances that match the filtering requirements.

:material-arrow-right-thin: Example: [generic-http](../api/serviceregistry/service-discovery-generic-http.md#lookup) | [generic-https](../api/serviceregistry/service-discovery-generic-http.md#lookup)<br />
:material-arrow-right-thin: Example: [generic-mqtt](../api/serviceregistry/service-discovery-generic-mqtt.md#lookup) | [generic-mqtts](../api/serviceregistry/service-discovery-generic-mqtt.md#lookup)

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

### general-management

Its purpose is to get some information about the hosting system’s behavior, such as log entries and configuration settings. The service is offered for administrative support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/general-management_sd.pdf) <br />
:material-api: [generic-http (IDD)](todo) | [generic-https (IDD)](todo) <br />
:material-api: [generic-mqtt (IDD)](todo) | [generic-mqtts (IDD)](todo) <br />
:material-tag: since: v5.0.0 

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

:fontawesome-solid-wrench: **authenticator.secret.keys**

Secret key for the authenticator servers when authentication policy is `outsourced`. The authenticator servers are able to register their authentication related services by providing their system name hashed with the associated secret key.

```
authenticator.secret.keys={\
	'<system-name>': '<secret-key>' \
}
```

:fontawesome-solid-wrench: **enable.blacklist.filter**

Enable/disable automatic service requester system name verification against to cloud level blacklist. Can be `true` or `false`.

:fontawesome-solid-wrench: **force.blacklist.filter**

Whether or not the service requests should be refused when the blacklist server is not responding. Can be `true` or `false`.

:fontawesome-solid-wrench: **enable.management.filter**

Enable or disable authorization for accessing the management services. Can be `true` of `false`.

:fontawesome-solid-wrench: **management.policy**

Way of authorizing the management service requester systems. Can be:
     
- `sysop-only`, when the authenticated requester system has _system-operator_ role that ensures overall management permission.
- `whitelist`, _sysop-only_ and when the authenticated requester system is whitelisted in the _management.whitelist_ configuration property that ensures overall management permission.
- `authorization`, _sysop-only_ and _whitelist_ and when the authenticated requester system has appropriate service permission according to the Authorization Core System.

:fontawesome-solid-wrench: **management.whitelist**

Name of the systems which can access to management services in case of `whitelist` policy is effective.

```
management.whitelist=<system-name-a>,<system-name-b>
```

:fontawesome-solid-wrench: **service.discovery.policy**

Behavior of the _service-discovery_ service. Can be:

- `open`, every existing service is directly discoverable.
- `restricted`, only the public core services are discoverable directly. The other ones must be orchestrated.

:fontawesome-solid-wrench: **service.discovery.direct.access**

Name of systems which always have direct access to the _service-discovery_ service. Only Core or Support systems should be listed here.

```
service.discovery.direct.access=<system-name-a>,<system-name-b>
```

:fontawesome-solid-wrench: **discovery.verbose**

Whether or not the _service-discovery_ service should provide system and device details as well and the _system-discovery_ service should provide device details as well. Can be `true` of `false`.

:fontawesome-solid-wrench: **service.discovery.interface.policy**

Way of handling non existing service interfaces during service registration. Can be:

- `restricted`, when only the already existing interface templates can be used.
- `extendable`, when new interface template will be created with all the provided properties as mandatory properties.
- `open`, when new interface template will be created without any further restrictions.

:fontawesome-solid-wrench: **allow.self.addressing**

Whether or not the registration of systems and devices with self-addressing IPv4, IPv6 and hostname addresses are allowed. In case of self-addressing addresses the IP packets cannot be directed from one device to another. Can be `true` of `false`.

:fontawesome-solid-wrench: **allow.non.routable.addressing**

Whether or not the registration of systems and devices with non-routable IPv4 and IPv6 addresses are allowed. In case of non-routable addresses the IP packets cannot be directed from one network to another. Can be `true` of `false`.

:fontawesome-solid-wrench: **service.address.alias**

Alias keys under which the service addresses can appear in the interface properties.

```
service.address.alias=<alias-1>,<alias-2>
```

:fontawesome-solid-wrench: **max.page.size**

Specifies the maximum number of records a page can contain in case of pageable service responses.

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

Related in CL-5.0.0

- [general](../general/changelogs/cl500.md#general)
- [arrowhead-common-utils](../general/changelogs/cl500.md#arrowhead-common-utils)
- [arrowhead-data-transfer-objects](../general/changelogs/cl500.md#arrowhead-data-transfer-objects)
- [arrowhead-serviceregistry](../general/changelogs/cl500.md#arrowhead-serviceregistry)
