# serviceOrchestration IDD (dynamic strategy)
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of serviceOrchestration, which provides runtime (late) binding between application systems. It’s implemented using protocol, encoding as stated in the following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestration – Service Description](../../assets/sd/5_0_0/service-orchestration_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### pull

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationRequest](../data-models/service-orchestration-request.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/pull

{
   "traceId":"<trace-id>",
   "authentication":"<authentication-data>",
   "responseTopic":"<response-topic>",
   "qosRequirement":<0|1|2>,
   "payload":{
      "serviceRequirement":{
         "serviceDefinition":"kelvinInfo",
         "operations":[
            "query-temperature"
         ],
         "versions":[
            
         ],
         "alivesAt":"2025-10-05T11:35:14Z",
         "metadataRequirements":[
            
         ],
         "interfaceTemplateNames":[
            "generic_https"
         ],
         "interfaceAddressTypes":[
            "HOSTNAME",
            "IPV4"
         ],
         "interfacePropertyRequirements":[
            
         ],
         "securityPolicies":[
            "TIME_LIMITED_TOKEN_AUTH"
         ],
         "preferredProviders":[
            
         ]
      },
      "orchestrationFlags":{
         "MATCHMAKING":"true",
         "ALLOW_TRANSLATION":"true",
         "ONLY_PREFERRED":"false",
         "ONLY_EXCLUSIVE":"false",
         "ALLOW_INTERCLOUD":"false",
         "ONLY_INTERCLOUD":"false"
      },
      "qosRequirements":{
         "max-latency-ms":"10"
      },
      "exclusivityDuration":600
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [ServiceOrchestrationResponse](../data-models/service-orchestration-response.md).

```
{
   "status":200,
   "traceId":"<trace-id>",
   "receiver":"TempConsumer",
   "payload":{
      "results":[
         {
            "serviceInstanceId":"TemperatureProvider2|kelvinInfo|1.0.0",
            "providerName":"TemperatureProvider2",
            "serviceDefinitition":"kelvinInfo",
            "version":"1.0.0",
            "cloudIdentitifer":"LOCAL",
            "aliveUntil":"2028-11-08T10:21:11Z",
            "exclusiveUntil":"2025-10-05T11:35:14Z",
            "metadata":{
               "margin-of-error":0.5
            },
            "interfaces":[
               {
                  "templateName":"generic_https",
                  "protocol":"https",
                  "policy":"TIME_LIMITED_TOKEN_AUTH",
                  "properties":{
                     "accessAddresses":[
                        "192.168.56.116",
                        "tp2.greenhouse.com"
                     ],
                     "accessPort":8080,
                     "operations":{
                        "query-temperature":{
                           "path":"/query",
                           "method":"GET"
                        }
                     },
                     "basePath":"/kelvin"
                  }
               }
            ],
            "authorizationTokens":{
               "TIME_LIMITED_TOKEN_AUTH":{
                  "query-temperature":{
                     "tokenType":"TIME_LIMITED_TOKEN",
                     "targetType":"SERVICE_DEF",
                     "token":"dsalefb521vdjkdsae633",
                     "expiresAt":"2025-10-05T11:35:14Z"
                  }
               }
            }
         }
      ],
      "warnings":[
         "part_time_exclusivity"
      ]
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":400,
   "traceId":"<trace-id>",
   "receiver":"TempConsumer",
   "payload":{
      "errorMessage":"QoS requirements are present, but QoS support is not enabled",
      "errorCode":400,
      "exceptionType":"INVALID_PARAMETER",
      "origin":"arrowhead/serviceorchestration/orchestration/pull"
   }
}
```

### subscribe

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationSubscriptionRequest](../data-models/service-orchestration-subscription-request.md).

```
Topic: arrowhead/serviceorchestration/orchestration/subscribe

{
   "traceId":"<trace-id>",
   "authentication":"<authentication-data>",
   "responseTopic":"<response-topic>",
   "qosRequirement":<0|1|2>,
   "payload":{
      "orchestrationRequest":{
         "serviceRequirement":{
            "serviceDefinition":"kelvinInfo",
            "operations":[
               "query-temperature"
            ],
            "versions":[
               
            ],
            "alivesAt":"2025-10-05T11:35:14Z",
            "metadataRequirements":[
               
            ],
            "interfaceTemplateNames":[
               "generic_https"
            ],
            "interfaceAddressTypes":[
               "HOSTNAME",
               "IPV4"
            ],
            "interfacePropertyRequirements":[
               
            ],
            "securityPolicies":[
               "TIME_LIMITED_TOKEN_AUTH"
            ],
            "preferredProviders":[
               
            ]
         },
         "orchestrationFlags":{
            "MATCHMAKING":"true",
            "ALLOW_TRANSLATION":"true",
            "ONLY_PREFERRED":"false",
            "ONLY_EXCLUSIVE":"false",
            "ALLOW_INTERCLOUD":"false",
            "ONLY_INTERCLOUD":"false"
         },
         "qosRequirements":{
            "max-latency-ms":"10"
         },
         "exclusivityDuration":600
      },
      "notifyInterface":{
         "protocol":"mqtt",
         "properties":{
            "topic":"arrowhead/orchestration-push"
         }
      },
      "duration":100000
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [UUID](../primitives.md#uuid).

```
{
   "status":200,
   "traceId":"<trace-id>",
   "receiver":"TempConsumer",
   "payload":"d4d61873-07db-4e93-a16e-9465852bdabf"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":400,
   "traceId":"<trace-id>",
   "receiver":"TempConsumer",
   "payload":{
      "errorMessage":"Unsupported notify protocol.",
      "errorCode":400,
      "exceptionType":"INVALID_PARAMETER",
      "origin":"arrowhead/serviceorchestration/orchestration/subscribe"
   }
}
```

### unsubscribe

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [UUID](../primitives.md#uuid).

```
Topic: arrowhead/serviceorchestration/orchestration/unsubscribe

{
   "traceId":"<trace-id>",
   "authentication":"<authentication-data>",
   "responseTopic":"<response-topic>",
   "qosRequirement":"<0|1|2>",
   "payload":"d4d61873-07db-4e93-a16e-9465852bdabf"
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and an existing subscription was removed and `204` if no matching record was found. 

```
{
  "status":<status-code>,
  "traceId":"<trace-id>",
  "receiver":"TempConsumer",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":400,
   "traceId":"<trace-id>",
   "receiver":"TempConsumer",
   "payload":{
      "errorMessage":"Invalid subscription id.",
      "errorCode":400,
      "exceptionType":"INVALID_PARAMETER",
      "origin":"arrowhead/serviceorchestration/orchestration/unsubscribe"
   }
}
```