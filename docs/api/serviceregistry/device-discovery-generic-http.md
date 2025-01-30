# device-discovery GENERIC-HTTP

## Overview

This section describes the GENERIC-HTTP service interface of device-discovery, which enables both
application and core/support systems to lookup, register and revoke devices on which the Local Cloud’s systems
are running. Device representation is not necessary for the base functionalities of a Local Cloud but in certain
use cases (e.g. enabling onboarding) is needed. It’s implemented using protocol, encoding as stated in the
following table:

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTP | 1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

This part provides the **Interface Design Description** (IDD) to the [device-discovery – Service Description](../../assets/sd/5_0_0/device-discovery_sd.pdf). For document further details about how this service is meant to be used, please consult that document.

## Interface Description

### register

The service operation **request** requires an authorization bearer header and a [DeviceRegistrationRequest](../data-models/device-registration-request.md)
JSON encoded body.

```
POST /serviceregistry/device-registry/register HTTP/1.1
Authorization: Bearer <authorization-info>

{
   "name":"thermometer2",
   "metadata":{
      "scales":[
         "Kelvin",
         "Celsius"
      ],
      "max-temperature":{
         "Kelvin":310,
         "Celsius":40
      },
      "min-temperature":{
         "Kelvin":260,
         "Celsius":-10
      }
   },
   "addresses":[      
      "address":"81:ef:1a:44:7a:f5"
   ]
}
```

The service operation **responses** with the status code `200 Ok` if called successfully and the device
entity is already existing or `201 Create` if the entity was newly created. The response also contains a
`DeviceRegistrationResponse` JSON encoded body.

```
{
   "name":"thermometer2",
   "metadata":{
      "scales":[
         "Kelvin",
         "Celsius"
      ],
      "max-temperature":{
         "Kelvin":310,
         "Celsius":40
      },
      "min-temperature":{
         "Kelvin":260,
         "Celsius":-10
      }
   },
   "addresses":[
      {
         "type":"MAC",
         "address":"81:ef:1a:44:7a:f5"
      }
   ],
   "createdAt":"2024-11-04T01:53:02Z",
   "updatedAt":"2024-11-04T01:53:02Z"
}
```
The **error codes** are, `400 Bad Request` if request is malformed, `403 Forbidden` if requester au-
thentication was unsuccessful, `401 Unauthorized` if the authenticated requester has no permission and
`500 Internal Server Error` if an unexpected error happens. The error response also contains an
`ErrorResponse` JSON encoded body.

```
{
    "errorMessage": "Device name is missing.",
    "errorCode": 400,
    "exceptionType: "INVALID_PARAMETER",
    "origin": "POST /serviceregistry/device-discovery/register"
}
```

### lookup

The service operation **request** requires an authorization bearer header and may optionally include a `DeviceLookup
Request` JSON encoded body.

```
POST /serviceregistry/device-registry/lookup HTTP/1.1
Authorization: Bearer <authorization-info>

{
   "deviceNames":[
      "thermometer2"
   ],
   "addresses":[
      "81:ef:1a:44:7a:f5"
   ],
   "addressType":"MAC",
   "metadataRequirementList":[
      {
         "max-temperature.Celsius":{
            "op":"LESS_THAN",
            "value":50
         }
      }
   ]
}
```

The service operation **responses** with the status code `200 Ok` if called successfully and with a `DeviceLookupResponse` JSON encoded body.

```
{
   "entries":[
      {
         "name":"thermometer2",
         "metadata":{
            "scales":[
               "Kelvin",
               "Celsius"
            ],
            "max-temperature":{
               "Kelvin":310,
               "Celsius":40
            },
            "min-temperature":{
               "Kelvin":260,
               "Celsius":-10
            }
         },
         "addresses":[
            {
               "type":"MAC",
               "address":"81:ef:1a:44:7a:f5"
            }
         ],
         "createdAt":"2024-11-04T01:53:02Z",
         "updatedAt":"2024-11-04T01:53:02Z"
      }
   ],
   "count":1
}
```

The error codes are, `400 Bad Request` if request is malformed, `403 Forbidden` if requester authentication was unsuccessful, `401 Unauthorized` if the authenticated requester has no permission and `500 Internal Server` Error if an unexpected error happens. The error response also contains an `ErrorResponse` JSON encoded body.

```
{
    "errorMessage": "Database operation error.",
    "errorCode": 500,
    "exceptionType: "INTERNAL_SERVER_ERROR",
    "origin": "POST /serviceregistry/device-discovery/lookup"
}
```

### revoke

The service operation **request** requires an authorization bearer header and a device `name` as path parameter.

```
DELETE /serviceregistry/device-discovery/revoke/thermometer2 HTTP/1.1
Authorization: Bearer <authorization-info>
```

The service operation **responses** with the status code `200 Ok` if called successfully and an existing device
entity was removed and `204 No Content` if no matching entity was found. The success response not contains
any response body.

The error codes are, `400 Bad Request` if request is malformed, `403 Forbidden` if requester authentication was unsuccessful, `401 Unauthorized` if the authenticated requester has no permission, `423 Locked` if entity is not removable and `500 Internal Server Error` if an unexpected error happens. The error response also contains an `ErrorResponse` JSON encoded body.

```
{
    "errorMessage": "Database operation error.",
    "errorCode": 500,
    "exceptionType: "INTERNAL_SERVER_ERROR",
    "origin": "DELETE /serviceregistry/device-discovery/lookup"
}
```