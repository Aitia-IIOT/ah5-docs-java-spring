# SimpleStoreServiceOrchestration

This Core System exists to find matching service instances according to the consumer’s specification within an Eclipse Arrowhead Local Cloud (LC).

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_2_0/SimpleStoreServiceOrchestration_sysd.pdf)

## Services

### serviceOrchestration

The purpose of this service is to get matching service instances. The service is offered for both application and Core/Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_2_0/service-orchestration_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/serviceorchestration/service-orchestration-generic-http_simple.md) | [generic_https (IDD)](../api/serviceorchestration/service-orchestration-generic-http_simple.md) <br />
:material-api: [generic_mqtt (IDD)](../api/serviceorchestration/service-orchestration-generic-mqtt_simple.md) | [generic_mqtts (IDD)](../api/serviceorchestration/service-orchestration-generic-mqtt_simple.md) <br />
:material-tag: since: v5.0.0 

Service metadata: <br />

```
{
    "orchestrationStrategy": "simpleStore",
    "unrestrictedDiscovery": true
}
```

**pull**

This service operation performs the orchestration process and returns the matching service instances.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-generic-http_simple.md#pull) | [generic_https](../api/serviceorchestration/service-orchestration-generic-http_simple.md#pull)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/serviceorchestration/service-orchestration-generic-mqtt_simple.md#pull) | [generic_mqtts](../api/serviceorchestration/service-orchestration-generic-mqtt_simple.md#pull)

**subscribe**

This service operation creates a subscription that can be triggered anytime to perform the orchestration process and push the matching service instances for the subscriber.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-generic-http_simple.md#subscribe) | [generic_https](../api/serviceorchestration/service-orchestration-generic-http_simple.md#subscribe)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/serviceorchestration/service-orchestration-generic-mqtt_simple.md#subscribe) | [generic_mqtts](../api/serviceorchestration/service-orchestration-generic-mqtt_simple.md#subscribe)

**unsubscribe**

This service operation removes a subscription.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-generic-http_simple.md#unsubscribe) | [generic_https](../api/serviceorchestration/service-orchestration-generic-http_simple.md#unsubscribe)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/serviceorchestration/service-orchestration-generic-mqtt_simple.md#unsubscribe) | [generic_mqtts](../api/serviceorchestration/service-orchestration-generic-mqtt_simple.md#unsubscribe)

-----

### generalManagement

Its purpose is to get some information about the hosting system’s behavior, such as log entries and configuration settings. The service is offered for administrative Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/general-management_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/general/general-management-generic-http.md) | [generic_https (IDD)](../api/general/general-management-generic-http.md) <br />
:material-api: [generic_mqtt (IDD)](../api/general/general-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/general/general-management-generic-mqtt.md) <br />
:material-tag: since: v5.0.0 

**get-log**

This service operation lists the log entries of the system that match the filtering requirements.

:material-arrow-right-thin: Example: [generic_http](../api/general/general-management-generic-http.md#get-log) | [generic_https](../api/general/general-management-generic-http.md#get-log)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/general/general-management-generic-mqtt.md#get-log) | [generic_mqtts](../api/general/general-management-generic-mqtt.md#get-log)

**get-config**

This service operation lists the current values of the specified configuration settings.

:material-arrow-right-thin: Example: [generic_http](../api/general/general-management-generic-http.md#get-config) | [generic_https](../api/general/general-management-generic-http.md#get-config)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/general/general-management-generic-mqtt.md#get-config) | [generic_mqtts](../api/general/general-management-generic-mqtt.md#get-config)

-----

### serviceOrchestrationPushManagement

The purpose of the service is to handle the push orchestration related data and activities centrally and in bulk. The service is offered for administrative Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/service-orchestration-push-management_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md) | [generic_https (IDD)](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md) <br />
:material-api: [generic_mqtt (IDD)](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md) | [generic_mqtts (IDD)](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md) <br />
:material-tag: since: v5.0.0 

**subscribe**

This service operation creates subscriptions in bulk for other consumer systems that can be triggered anytime to perform the orchestration process and push newly orchestrated matching service instances to the subscribed consumers.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md#subscribe) | [generic_https](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md#subscribe)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md#subscribe) | [generic_mqtts](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md#subscribe)

**unsubscribe**

This service operation removes subscriptions in bulk.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md#unsubscribe) | [generic_https](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md#unsubscribe)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md#unsubscribe) | [generic_mqtts](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md#unsubscribe)

**trigger**

This service operation initiates service orchestration processes based on consumer system names, subscription identifiers or a creator system name.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md#trigger) | [generic_https](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md#trigger)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md#trigger) | [generic_mqtts](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md#trigger)

**query**

This service operation lists the existing subscriptions that match the filtering requirements.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md#query) | [generic_https](../api/serviceorchestration/service-orchestration-push-management-generic-http_simple.md#query)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md#query) | [generic_mqtts](../api/serviceorchestration/service-orchestration-push-management-generic-mqtt_simple.md#query)

-----

### serviceOrchestrationSimpleStoreManagement

The purpose of the service is to manage the peer-to-peer orchestration rules. These rules associate consumer systems to service instances and can be used during orchestration process in case of simple-store strategy.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_2_0/service-orchestration-simple-store-management_sd.pdf) <br />

-----

### serviceOrchestrationHistoryManagement

The purpose of the service is to gather information about the performed orchestration processes.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/service-orchestration-history-management_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/serviceorchestration/service-orchestration-history-management-generic-http.md) | [generic_https (IDD)](../api/serviceorchestration/service-orchestration-history-management-generic-http.md) <br />
:material-api: [generic_mqtt (IDD)](../api/serviceorchestration/service-orchestration-history-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/serviceorchestration/service-orchestration-history-management-generic-mqtt.md) <br />
:material-tag: since: v5.0.0 

**query**

This service operation lists the existing orchestration job records that match the filtering requirements.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-history-management-generic-http.md#query) | [generic_https](../api/serviceorchestration/service-orchestration-history-management-generic-http.md#query)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/serviceorchestration/service-orchestration-history-management-generic-mqtt.md#query) | [generic_mqtts](../api/serviceorchestration/service-orchestration-history-management-generic-mqtt.md#query)

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
- `authorization`, _sysop-only_ and _whitelist_ and when the authenticated requester system has appropriate service permission according to the ConsumerAuthorization Core system.

:fontawesome-solid-wrench: **management.whitelist**

Name of the systems which can access to management services in case of `whitelist` policy is effective.

```
management.whitelist=<system-name-a>,<system-name-b>
```

:fontawesome-solid-wrench: **orchestration.history.max.age**

Specifies after how many days the orchestration history records can be automatically deleted.

:fontawesome-solid-wrench: **cleaner.job.interval**

Specifies in milisec, that how often the expired subscriptions and the orchestration history records can be automatically deleted.

:fontawesome-solid-wrench: **push.orchestration.max.thread**

Specifies the maximum number of threads that can be allocated to process push orchestration tasks.