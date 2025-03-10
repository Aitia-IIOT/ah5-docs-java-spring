# system-discovery GENERIC-HTTP

## Overview

This page describes the GENERIC-HTTP and GENERIC-HTTPS service interface of system-discovery, which enables both
application and core/support systems to lookup, register and revoke systems that are part of the Local Cloud.  System representation is mandatory for the base functionalities of a Local Cloud, e.g. the systems have to be registered in order to interact with each other. The interfaces are implemented using protocol, encoding as stated in the following tables:

## Interface Description

**GENERIC-HTTP**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTP | 1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**GENERIC-HTTPS**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTPS | 1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [system-discovery – Service Description](../../assets/sd/5_0_0/system-discovery_sd.pdf). For further details about how this service is meant to be used, please consult that document.

### register

The service operation **request** requires an authorization bearer header and a [SystemRegistrationRequest](../data-models/system-registration-request.md)
JSON encoded body.

```
POST /serviceregistry/system-discovery/register HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "metadata": {
    "scales": ["Kelvin", "Celsius"],
    "location": {"side": "North", "block": 2},
    "indoor": true
  },
  "version": "",
  "addresses": [
    "192.168.56.116",
    "tp2.greenhouse.com"
  ],
  "deviceName": "thermometer2"
}
```

The service operation **responds** with the status code `200` if called successfully and the system
entity is already existing or `201` if the entity was newly created. The response also contains a
[SystemRegistrationResponse](../data-models/system-registration-response.md) JSON encoded body.

```
{
  "name": "temperature-provider2",
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
    "name": "thermometer2",
    "metadata": {
      "scales": [
        "Kelvin",
        "Celsius"
      ],
      "max-temperature": {
        "Kelvin": 310,
        "Celsius": 40
      },
      "min-temperature": {
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
  "createdAt": "2024-11-08T10:21:10.950683800Z",
  "updatedAt": "2024-11-08T10:21:10.950683800Z"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Device names do not exist: thermometer2",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/system-discovery/register"
}
```

### lookup

The service operation **request** requires an authorization bearer header. The URI can contain an optional query parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device information also returns (only if the provider supports it). The request may optionally include a [SystemLookupRequest](../data-models/system-lookup-request.md) JSON encoded body.

```
POST /serviceregistry/system-discovery/lookup?verbose=<verbose-value> HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "systemNames": [
  ],
  "addresses": [
  ],
  "addressType": "",
  "metadataRequirementList": [
  ],
  "versions": [
  ],
  "deviceNames": [
    "thermometer2"
  ]
}
```

The service operation **responds** with the status code `200` if called successfully and with a [SystemLookupResponse](../data-models/system-lookup-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "name": "temperature-provider1",
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
        "name": "thermometer2"
      },
      "createdAt": "2025-02-27T18:32:45Z",
      "updatedAt": "2025-02-27T18:32:45Z"
    }
  ],
  "count": 1
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid address type: IPV5",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/system-discovery/lookup"
}
```

### revoke

The service operation **request** only requires an authorization bearer header. The name of the system to be revoked will be identified during authentication.

```
DELETE /serviceregistry/system-discovery/revoke HTTP1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with the status code `200` if called successfully and an existing system
entity was removed and `204` if no matching entity was found. The success response does not contain
any response body.

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "No authorization header has been provided",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```