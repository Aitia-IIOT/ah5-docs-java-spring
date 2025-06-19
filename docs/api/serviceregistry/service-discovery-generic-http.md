# serviceDiscovery IDD
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of serviceDiscovery, which enables both
application and Core/Support systems to register and revoke their service instances to/from the Local Cloud. It also enables to lookup for service instances. Service and service instance representation is mandatory for the base functionalities of a Local Cloud therefore it is an integral part of the implementation of the requirements in ServiceRegistry Core System. An example of this interaction is when a provider registers its service instances to offer them to other systems in the Local Cloud. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry. The interfaces are implemented using protocol, encoding as stated in the following tables:

## Interface Description

**generic_http**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTP | 1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**generic_https**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTPS | 1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [serviceDiscovery – Service Description](../../assets/sd/5_0_0/service-discovery_sd.pdf). For further details about how this service is meant to be used, please consult that document.

### register

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceRegistrationRequest](../data-models/service-registration-request.md)
JSON encoded body.

```
POST /serviceregistry/service-discovery/register HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```

The service operation **responds** with the status code `201` if the service instance entity was created. The response also contains a
[ServiceRegistrationResponse](../data-models/service-registration-response.md) JSON encoded body.

```
{
  "instanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
  "provider": {
    "name": "TemperatureProvider2",
    "metadata": {
      "scales": [
        "Kelvin",
        "Celsius"
      ],
      "location": {
        "side": "North",
        "block": 2
      },
      "indoor": true
    },
    "version": "1.0.0",
    "addresses": [
      {
        "type": "IPV4",
        "address": "192.168.56.116"
      },
      {
        "type": "HOSTNAME",
        "address": "tp2.greenhouse.com"
      }
    ],
    "device": {
      "name": "THERMOMETER2",
      "metadata": {
        "scales": [
          "Kelvin",
          "Celsius"
        ],
        "maxTemperature": {
          "Kelvin": 310,
          "Celsius": 40
        },
        "minTemperature": {
          "Kelvin": 260,
          "Celsius": -10
        }
      },
      "addresses": [
        {
          "type": "MAC",
          "address": "81:ef:1a:44:7a:f5"
        }
      ],
      "createdAt": "2024-11-04T01:53:02Z",
      "updatedAt": "2024-11-04T01:53:02Z"
    },
    "createdAt": "2024-11-08T10:21:11Z",
    "updatedAt": "2024-11-08T10:21:11Z"
  },
  "serviceDefinition": {
    "name": "kelvinInfo",
    "createdAt": "2024-11-08T11:24:43Z",
    "updatedAt": "2024-11-08T11:24:43Z"
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
  "createdAt": "2024-11-19T12:00:07.959849300Z",
  "updatedAt": "2024-11-19T12:00:07.959849300Z"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Expiration time has an invalid time format",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/service-discovery/register"
}
```

### lookup

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http). The URI can contain an optional query parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device and system information also returns (only if the provider supports it). The request requires a [ServiceLookupRequest](../data-models/service-lookup-request.md) JSON encoded body.

```
POST /serviceregistry/service-discovery/lookup?verbose=<verbose-value> HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "instanceIds": [
  ],
  "providerNames": [
    "TemperatureProvider2"
  ],
  "serviceDefinitionNames": [
    "alertService"
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
```

The service operation **responds** with the status code `200` if called successfully and with a [ServiceLookupResponse](../data-models/service-lookup-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "instanceId": "TemperatureProvider2|alertService|1.0.0",
      "provider": {
        "name": "TemperatureProvider2",
        "metadata": {
          "scales": [
            "Kelvin",
            "Celsius"
          ],
          "location": {
            "side": "North",
            "block": 2
          },
          "indoor": true
        },
        "version": "1.0.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.56.116"
          },
          {
            "type": "HOSTNAME",
            "address": "tp2.greenhouse.com"
          }
        ],
        "device": {
          "name": "THERMOMETER2",
          "metadata": {
            "scales": [
              "Kelvin",
              "Celsius"
            ],
            "maxTemperature": {
              "Kelvin": 310,
              "Celsius": 40
            },
            "minTemperature": {
              "Kelvin": 260,
              "Celsius": -10
            }
          },
          "addresses": [
            {
              "type": "MAC",
              "address": "81:ef:1a:44:7a:f5"
            }
          ],
          "createdAt": "2024-11-04T01:53:02Z",
          "updatedAt": "2024-11-04T01:53:02Z"
        },
        "createdAt": "2024-11-08T10:21:11Z",
        "updatedAt": "2024-11-08T10:21:11Z"
      },
      "serviceDefinition": {
        "name": "alertService",
        "createdAt": "2024-11-08T15:23:10Z",
        "updatedAt": "2024-11-08T15:23:10Z"
      },
      "version": "1.0.0",
      "expiresAt": "2025-01-01T00:00:00Z",
      "metadata": {
        "maxDelay": {
          "value": 15,
          "unit": "sec"
        }
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
              "subscribe": {
                "path": "/subscribe",
                "method": "POST"
              },
              "unsubscribe": {
                "path": "/unsubscribe",
                "method": "DELETE"
              },
              "set-threshold": {
                "path": "/threshold",
                "method": "POST"
              }
            },
            "basePath": "/alert"
          }
        }
      ],
      "createdAt": "2024-11-19T17:08:48Z",
      "updatedAt": "2024-11-19T17:08:48Z"
    }
  ],
  "count": 1
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "One of the following filters must be used: 'instanceIds', 'providerNames', 'serviceDefinitionNames'",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/service-discovery/lookup"
}
```

### revoke

The service operation **request**  requires an [identity related header or certificate](../authentication_policy.md/#http), and a [ServiceInstanceID](../primitives.md#serviceinstanceid) as path parameter, which is a unique identifier of the service instance to be deleted.

```
DELETE /serviceregistry/service-discovery/revoke/TemperatureProvider1%7CcelsiusInfo%7C1.0.0 HTTP1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with the status code `200` if called successfully and an existing service instance entity was removed and `204` if no matching entity was found. The success response does not contain any response body.

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "No authorization header has been provided",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```