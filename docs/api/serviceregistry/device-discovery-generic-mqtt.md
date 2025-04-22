# device-discovery IDD
**GENERIC-MQTT & GENERIC-MQTTS** 

## Overview

This page describes the GENERIC-MQTT and GENERIC-MQTTS service interface of device-discovery, which enables both
application and core/support systems to lookup, register and revoke devices on which the Local Cloud’s systems
are running. Device representation is not necessary for the base functionalities of a Local Cloud but in certain
use cases (e.g. enabling onboarding) is needed. It’s implemented using protocol, encoding as stated in the
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

Hereby the **Interface Design Description** (IDD) is provided to the [device-discovery – Service Description](../../assets/sd/5_0_0/device-discovery_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### register

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [DeviceRegistrationRequest](../data-models/device-registration-request.md).

```
Topic: arrowhead/serviceregistry/device-discovery/register

{
   "traceId":"<trace-id>",
   "authentication":"<identity-info>",
   "responseTopic":"<response-topic>",
   "qosRequirement":"<0|1|2>",
   "payload":{
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
         "81:ef:1a:44:7a:f5"
      ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and the device entity is already existing or `201` if the entity was newly created. The response template payload is a
[DeviceRegistrationResponse](../data-models/device-registration-response.md).

```
{
   "status":"<status-code>",
   "traceId":"<trace-id>",
   "receiver":"<receiver-system-identifier>",
   "payload":{
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
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":"<status-code>",
   "traceId":"<trace-id>",
   "receiver":"<receiver-system-identifier>",
   "payload":{
      "errorMessage":"Device name is missing.",
      "errorCode":400,
      "exceptionType":"INVALID_PARAMETER",
      "origin":"arrowhead/serviceregistry/device-discovery/register"
   }
}
```

### lookup

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a  [DeviceLookupRequest](../data-models/device-lookup-request.md).

```
Topic: arrowhead/serviceregistry/device-discovery/lookup

{
   "traceId":"<trace-id>",
   "authentication":"<identity-info>",
   "responseTopic":"<response-topic>",
   "qosRequirement":"<0|1|2>",
   "payload":{
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
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [DeviceLookupResponse](../data-models/device-lookup-response.md).

```
{
   "status":"<status-code>",
   "traceId":"<trace-id>",
   "receiver":"<receiver-system-identifier>",
   "payload":{
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
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens.  In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":"<status-code>",
   "traceId":"<trace-id>",
   "receiver":"<receiver-system-identifier>",
   "payload":{
      "errorMessage":"Database operation error.",
      "errorCode":500,
      "exceptionType":"INTERNAL_SERVER_ERROR",
      "origin":"arrowhead/serviceregistry/device-discovery/lookup"
   }
}
```

### revoke

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is the `device name`.

```
Topic: arrowhead/serviceregistry/device-discovery/revoke

{
   "traceId":"<trace-id>",
   "authentication":"<identity-info>",
   "responseTopic":"<response-topic>",
   "qosRequirement":"<0|1|2>",
   "payload":"thermometer2"
}
```
The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and an existing device entity was removed and `204` if no matching entity was found. 

```
{
   "status":"<status-code>",
   "traceId":"<trace-id>",
   "receiver":"<receiver-system-identifier>"
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `423` if the entity is not removable and `500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":"<status-code>",
   "traceId":"<trace-id>",
   "receiver":"<receiver-system-identifier>",
   "payload":{
      "errorMessage":"Database operation error.",
      "errorCode":500,
      "exceptionType":"INTERNAL_SERVER_ERROR",
      "origin":"arrowhead/serviceregistry/device-discovery/revoke"
   }
}
```