# DynamicServiceOrchestration

This Core System exists to find matching service instances according to the consumer’s specification within an Eclipse Arrowhead Local Cloud (LC) and optionally, in other Arrowhead clouds by collaborating with other core/support Systems. 

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_0_0/dynamicserviceorchestration_sysd.pdf)

## Services

### serviceOrchestration

The purpose of this service is to get matching service instances. The service is offered for both application and Core/Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/service-orchestration_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/serviceorchestration/service-orchestration-generic-http_dynamic.md) | [generic_https (IDD)](../api/serviceorchestration/service-orchestration-generic-http_dynamic.md) <br />
:material-api: [generic_mqtt (IDD)](todo) | [generic_mqtts (IDD)](todo) <br />
:material-tag: since: v5.0.0 

**pull**

This service operation performs the orchestration process and returns the matching service instances.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-generic-http_dynamic.md#pull) | [generic_https](../api/serviceorchestration/service-orchestration-generic-http_dynamic.md#pull)<br />
:material-arrow-right-thin: Example: [generic_mqtt](todo) | [generic_mqtts](todo)

**subscribe**

This service operation creates a subscription that can be triggered anytime to perform the orchestration process and push the matching service instances for the subscriber.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-generic-http_dynamic.md#subscribe) | [generic_https](../api/serviceorchestration/service-orchestration-generic-http_dynamic.md#subscribe)<br />
:material-arrow-right-thin: Example: [generic_mqtt](todo) | [generic_mqtts](todo)

**unsubscribe**

This service operation removes a subscription.

:material-arrow-right-thin: Example: [generic_http](../api/serviceorchestration/service-orchestration-generic-http_dynamic.md#unsubscribe) | [generic_https](../api/serviceorchestration/service-orchestration-generic-http_dynamic.md#unsubscribe)<br />
:material-arrow-right-thin: Example: [generic_mqtt](todo) | [generic_mqtts](todo)

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

Coming soon.

-----

### serviceOrchestrationLockManagement

Coming soon.

-----

### serviceOrchestrationHistoryManagement

Coming soon.

## Configuration

Coming soon.

## Changelog

#### :material-tag: v5.0.0 

Related in CL-5.0.0

- [general](../general/changelogs/cl500.md#general)
- [arrowhead-common-utils](../general/changelogs/cl500.md#arrowhead-common-utils)
- [arrowhead-data-transfer-objects](../general/changelogs/cl500.md#arrowhead-data-transfer-objects)
- [arrowhead-serviceorchestration-dynamic](../general/changelogs/cl500.md#arrowhead-serviceorchestration-dynamic)