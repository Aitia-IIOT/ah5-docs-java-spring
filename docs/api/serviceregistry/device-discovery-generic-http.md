# deviceDiscovery IDD
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of deviceDiscovery, which enables both
application and Core/Support systems to lookup, register and revoke devices on which the Local Cloud’s systems
are running. Device representation is not necessary for the base functionalities of a Local Cloud but in certain
use cases (e.g. enabling onboarding) is needed. It’s implemented using protocol, encoding as stated in the
following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [deviceDiscovery – Service Description](../../assets/sd/5_0_0/device-discovery_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### register

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [DeviceRegistrationRequest](../data-models/device-registration-request.md)
JSON encoded body.

```
POST /serviceregistry/device-discovery/register HTTP/1.1
Authorization:  Bearer <identity-info>

{
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
      "81:ef:1a:44:7a:f5"
   ]
}
```

The service operation **responds** with the status code `200` if called successfully and the device entity is already existing or `201` if the entity was newly created. The response also contains a
[DeviceRegistrationResponse](../data-models/device-registration-response.md) JSON encoded body.

```
{
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
}
```
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
    "errorMessage":  "Device name is missing.",
    "errorCode":  400,
    "exceptionType":  "INVALID_PARAMETER",
    "origin":  "POST /serviceregistry/device-discovery/register"
}
```

### lookup

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and may optionally include a [DeviceLookupRequest](../data-models/device-lookup-request.md) JSON encoded body.

```
POST /serviceregistry/device-discovery/lookup HTTP/1.1
Authorization:  Bearer <identity-info>

{
   "deviceNames": [
      "THERMOMETER2"
   ],
   "addresses": [
      "81:ef:1a:44:7a:f5"
   ],
   "addressType": "MAC",
   "metadataRequirementList": [
      {
         "maxTemperature.Celsius": {
            "op": "LESS_THAN",
            "value": 50
         }
      }
   ]
}
```

The service operation **responds** with the status code `200` if called successfully and with a [DeviceLookupResponse](../data-models/device-lookup-response.md) JSON encoded body.

```
{
   "entries": [
      {
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
      }
   ],
   "count": 1
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
    "errorMessage":  "Database operation error.",
    "errorCode":  500,
    "exceptionType":  "INTERNAL_SERVER_ERROR",
    "origin":  "POST /serviceregistry/device-discovery/lookup"
}
```

### revoke

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a device [`name`](../primitives.md#devicename) as path parameter.

```
DELETE /serviceregistry/device-discovery/revoke/THERMOMETER2 HTTP/1.1
Authorization:  Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully and an existing device
entity was removed and `204` if no matching entity was found. The success response does not contain any response body.

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `423` if entity is not removable and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
    "errorMessage":  "Database operation error.",
    "errorCode":  500,
    "exceptionType":  "INTERNAL_SERVER_ERROR",
    "origin":  "DELETE /serviceregistry/device-discovery/lookup"
}
```