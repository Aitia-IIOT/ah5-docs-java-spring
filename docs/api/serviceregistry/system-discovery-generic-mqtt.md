# systemDiscovery IDD
**generic_mqtt & generic_mqtts** 

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of systemDiscovery, which enables both application and Core/Support systems to lookup, register and revoke systems that are part of the Local Cloud.  System representation is mandatory for the base functionalities of a Local Cloud, e.g. the systems have to be registered in order to interact with each other.

Hereby the **Interface Design Description** (IDD) is provided to the [systemDiscovery – Service Description](../../assets/sd/5_0_0/system-discovery_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### register

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [SystemRegistrationResponse](../data-models/system-registration-response.md).

```
Topic:  arrowhead/serviceregistry/system-discovery/register

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "metadata":  {
       "scales":  ["kelvin", "celsius"],
       "location":  {"side":  "North", "block":  2},
       "indoor":  true
     },
     "version":  "",
     "addresses":  [
       "192.168.56.116",
       "tp2.greenhouse.com"
     ],
     "deviceName":  "THERMOMETER2"
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and the system entity is already existing or `201` if the entity was newly created. The response template payload is a
[SystemRegistrationResponse](../data-models/system-registration-response.md).

```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "name": "TemperatureProvider2",
    "metadata": {
      "scales": [
        "kelvin",
        "celsius"
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
          "kelvin",
          "celsius"
        ],
        "maxTemperature": {
          "kelvin": 310,
          "celsius": 40
        },
        "minTemperature": {
          "kelvin": 260,
          "celsius": -10
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
   "receiver": "<receiver-system-identifier>",
   "payload": {
      "errorMessage": "Device names do not exist:  THERMOMETER2",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": arrowhead/serviceregistry/system-discovery/register
   }
}
```

### lookup

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an optional [SystemLookupRequest](../data-models/system-lookup-request.md). The params can contain a [KeyValuePair](../primitives.md/#keyvaluepair) with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device information also returns (only if the provider supports it).

```
Topic:  arrowhead/serviceregistry/system-discovery/lookup

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "params": {"verbose": false},
   "payload": {
     "systemNames":  [
     ],
     "addresses":  [
     ],
     "addressType":  "",
     "metadataRequirementList":  [
     ],
     "versions":  [
     ],
     "deviceNames":  [
       "THERMOMETER2"
     ]
   }
}   
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [SystemLookupResponse](../data-models/system-lookup-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "entries":  [
       {
         "name":  "TemperatureProvider1",
         "metadata":  {
            "scales":  [
              "kelvin",
              "celsius"
            ],
            "location":  {
              "side":  "North",
              "block":  2
            },
            "indoor":  true
         },
         "version":  "1.0.0",
         "addresses":  [
           {
             "type":  "IPV4",
             "address":  "192.168.56.116"
           },
           {
             "type":  "HOSTNAME",
             "address":  "tp2.greenhouse.com"
           }
         ],
         "device":  {
           "name":  "THERMOMETER2"
         },
         "createdAt":  "2025-02-27T18:32:45Z",
         "updatedAt":  "2025-02-27T18:32:45Z"
       }
     ],
     "count":  1
   }
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
      "errorMessage": "Invalid address type:  IPV5",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceregistry/system-discovery/lookup"
   }
}

```

### revoke

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt). The name of the system to be revoked will be identified during authentication.

```
Topic:  arrowhead/serviceregistry/system-discovery/revoke

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": ""
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and an existing system entity was removed and `204` if no matching entity was found. 

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>"
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 401,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
      "errorMessage": "No authentication info has been provided",
      "errorCode": 401,
      "exceptionType": "AUTH"
   }
}
```