# serviceDiscovery IDD
**generic_mqtt & generic_mqtts** 

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of serviceDiscovery, which enables both application and Core/Support systems to register and revoke their service instances to/from the Local Cloud. It also enables to lookup for service instances. Service and service instance representation is mandatory for the base functionalities of a Local Cloud therefore it is an integral part of the implementation of the requirements in ServiceRegistry Core System. An example of this interaction is when a provider registers its service instances to offer them to other systems in the Local Cloud. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry. It’s implemented using protocol, encoding as stated in the
following tables:

**generic_mqtt**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**generic_mqtts**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [serviceDiscovery – Service Description](../../assets/sd/5_0_0/service-discovery_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### register

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceRegistrationRequest](../data-models/service-registration-request.md). 

```
Topic: arrowhead/serviceregistry/service-discovery/register

{
  "traceId": "<trace-id>",
  "authentication": "<authentication-data>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "serviceDefinitionName": "kelvinInfo",
    "version": "",
    "expiresAt": "2030-01-01T00:00:00Z",
    "metadata": {
      "marginOfError": 0.5
    },
    "interfaces": [
      {
        "templateName": "generic_http",
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
    "instanceId": "TemperatureProvider1|kelvinInfo|1.0.0",
    "provider": {
      "name": "TemperatureProvider1",
      "metadata": {
        "type": "temperature",
        "scales": [
          "kelvin",
          "celsius"
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
      "name": "kelvinInfo",
      "createdAt": "2025-03-16T23:31:20Z",
      "updatedAt": "2025-03-16T23:31:20Z"
    },
    "version": "1.0.0",
    "expiresAt": "2030-01-01T00:00:00Z",
    "metadata": {
      "marginOfError": 0.5
    },
    "interfaces": [
      {
        "templateName": "generic_http",
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
  "payload": {
    "errorMessage": "System not exists: TempProvider",
	"errorCode": 400,
	"exceptionType": "INVALID_PARAMETER",
	"origin": "arrowhead/serviceregistry/service-discovery/register"
}
```

### lookup

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt). The MQTTRequestTemplate can contain an optional parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device and system information also returns (only if the provider supports it). The payload of the MQTTRequestTemplate is a [ServiceLookupRequest](../data-models/service-lookup-request.md).

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
      "TemperatureProvider1"
    ],
    "serviceDefinitionNames": [
      "kelvinInfo"
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
        "instanceId": "TemperatureProvider1|kelvinInfo|1.0.0",
        "provider": {
          "name": "TemperatureProvider1",
          "metadata": {
            "type": "temperature",
            "scales": [
              "kelvin",
              "celsius"
            ],
            "customizable": false
          },
          "version": "2.1.0",
          "createdAt": "2025-03-09T18:03:26Z",
          "updatedAt": "2025-03-09T18:03:26Z"
        },
        "serviceDefinition": {
          "name": "kelvinInfo",
          "createdAt": "2025-03-16T23:31:20Z",
          "updatedAt": "2025-03-16T23:31:20Z"
        },
        "version": "1.0.0",
        "expiresAt": "2030-01-01T00:00:00Z",
        "metadata": {
          "marginOfError": 0.5
        },
        "interfaces": [
          {
            "templateName": "generic_http",
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
  "payload": {
    "errorMessage": "One of the following filters must be used: 'instanceIds', 'providerNames', 'serviceDefinitionNames'",
	"errorCode": 400,
	"exceptionType": "INVALID_PARAMETER",
	"origin": "arrowhead/serviceregistry/service-discovery/lookup"
  }
}
```

### revoke

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceInstanceID](../primitives.md#serviceinstanceid). This is a unique identifier of the service instance to be deleted.

```
Topic: arrowhead/serviceregistry/service-discovery/revoke

{
  "traceId": "<trace-id>",
  "authentication": "<authentication-data>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": "TemperatureProvider1|kelvinInfo|1.0.0"
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and an existing service instance entity was removed and `204` if no matching entity was found. 

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "TemperatureProvider1",
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
  "payload": {
    "errorMessage": ""Revoking other systems' service is forbidden"",
	"errorCode": 403,
	"exceptionType": "FORBIDDEN",
	"origin": "arrowhead/serviceregistry/service-discovery/revoke"
  }
}
```