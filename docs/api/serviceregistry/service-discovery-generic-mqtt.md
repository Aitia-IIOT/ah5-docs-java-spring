# service-discovery IDD
**GENERIC-MQTT & GENERIC-MQTTS** 

## Overview

This page describes the GENERIC-MQTT and GENERIC-MQTTS service interface of service-discovery, which enables both application and core/support systems to lookup, register and revoke their service instances to/from the Local Cloud. It also enables to lookup for service instances. Service and service instance representation is mandatory for the base functionalities of a Local Cloud therefore it is an integral part of the implementation of the requirements in Service Registry Core System. An example of this interaction is when a provider registers its service instances to offer them to other systems in the Local Cloud. To enable other systems to use, to consume it, this service needs to be offered through the Service Registry. It’s implemented using protocol, encoding as stated in the
following tables:

**GENERIC-MQTT**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**GENERIC-MQTTS**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [service-discovery – Service Description](../../assets/sd/5_0_0/service-discovery_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### register

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the payload is a [ServiceRegistrationRequest](../data-models/service-registration-request.md).

```
Topic: arrowhead/serviceregistry/service-discovery/register

{
  "traceId": "<trace-id>",
  "authentication": "<authentication-data>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "serviceDefinitionName": "kelvin-info",
    "version": "",
    "expiresAt": "2030-01-01T00:00:00Z",
    "metadata": {
      "margin-of-error": 0.5
    },
    "interfaces": [
      {
        "templateName": "generic-http",
        "protocol": "http",
        "policy": "NONE",
        "properties": {
          "accessAddresses": ["192.168.56.116", "tp2.greenhouse.com"],
          "accessPort": 8080,
          "basePath": "/kelvin",
          "operations": {"query-temperature": { "method": "GET", "path": "/query"} }
        }
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [ServiceRegistrationResponse](../data-models/service-registration-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "instanceId": "temperature-provider1::kelvin-info::1.0.0",
    "provider": {
      "name": "temperature-provider1",
      "metadata": {
        "type": "temperature",
        "scales": [
          "Kelvin",
          "Celsius"
        ],
        "customizable": false
      },
      "version": "2.1.0",
      "addresses": [
        {
          "type": "HOSTNAME",
          "address": "tp1.greenhouse.com"
        },
        {
          "type": "IPV4",
          "address": "192.168.66.1"
        }
      ],
      "createdAt": "2025-03-09T18:03:26Z",
      "updatedAt": "2025-03-09T18:03:26Z"
    },
    "serviceDefinition": {
      "name": "kelvin-info",
      "createdAt": "2025-03-16T23:31:20Z",
      "updatedAt": "2025-03-16T23:31:20Z"
    },
    "version": "1.0.0",
    "expiresAt": "2030-01-01T00:00:00Z",
    "metadata": {
      "margin-of-error": 0.5
    },
    "interfaces": [
      {
        "templateName": "generic-http",
        "protocol": "http",
        "policy": "NONE",
        "properties": {
          "accessAddresses": [
            "192.168.56.116",
            "tp2.greenhouse.com"
          ],
          "accessPort": 8080,
          "operations": {
            "query-temperature": {
              "path": "/query",
              "method": "GET"
            }
          },
          "basePath": "/kelvin"
        }
      }
    ],
    "createdAt": "2025-03-16T23:34:17.785249400Z",
    "updatedAt": "2025-03-16T23:34:17.785249400Z"
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "temp-provider",
  "payload": "System not exists: temp-provider"
}
```

### lookup

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message. The MQTTRequestTemplate contains a parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device and system information also returns (only if the provider supports it). The payload of the MQTTRequestTemplate is a [ServiceLookupRequest](../data-models/service-lookup-request.md).

```
Topic: arrowhead/serviceregistry/service-discovery/lookup

{
  "traceId": ""<trace-id>",
  "authentication": "<authentication-data>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "params": {"verbose": false},
  "payload": {
    "instanceIds": [
    ],
    "providerNames": [
      "temperature-provider1"
    ],
    "serviceDefinitionNames": [
      "kelvin-info"
    ],
    "versions": [
      "1.0.0"
    ],
    "alivesAt": "",
    "metadataRequirementsList": [
    ],
    "interfaceTemplateNames": [
    ],
    "interfacePropertyRequirementsList": [
    ],
    "policies": [
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [ServiceLookupResponse](../data-models/service-lookup-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "instanceId": "temperature-provider1::kelvin-info::1.0.0",
        "provider": {
          "name": "temperature-provider1",
          "metadata": {
            "type": "temperature",
            "scales": [
              "Kelvin",
              "Celsius"
            ],
            "customizable": false
          },
          "version": "2.1.0",
          "createdAt": "2025-03-09T18:03:26Z",
          "updatedAt": "2025-03-09T18:03:26Z"
        },
        "serviceDefinition": {
          "name": "kelvin-info",
          "createdAt": "2025-03-16T23:31:20Z",
          "updatedAt": "2025-03-16T23:31:20Z"
        },
        "version": "1.0.0",
        "expiresAt": "2030-01-01T00:00:00Z",
        "metadata": {
          "margin-of-error": 0.5
        },
        "interfaces": [
          {
            "templateName": "generic-http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.56.116",
                "tp2.greenhouse.com"
              ],
              "accessPort": 8080,
              "operations": {
                "query-temperature": {
                  "path": "/query",
                  "method": "GET"
                }
              },
              "basePath": "/kelvin"
            }
          }
        ],
        "createdAt": "2025-03-16T23:34:18Z",
        "updatedAt": "2025-03-16T23:34:18Z"
      }
    ],
    "count": 1
  }
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens.  In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "trace1",
  "receiver": "<receiver-system-identifier>",
  "payload": "Alive time has an invalid time format"
}
```

### revoke

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the payload is a [Name](../primitives.md#name). This name is a unique identifier of the service instance to be deleted.

```
Topic: arrowhead/serviceregistry/service-discovery/revoke

{
  "traceId": "<trace-id>",
  "authentication": "<authentication-data>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": "temperature-provider1::kelvin-info::1.0.0"
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and an existing service instance entity was removed and `204` if no matching entity was found. 

```
{
  "status": <status-code>,
  "traceId": "<trace-id>",
  "receiver": "temperature-provider1",
  "payload": ""
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 403,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": "Revoking other systems' service is forbidden"
}
```