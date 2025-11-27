# serviceOrchestration IDD (dynamic strategy)
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of serviceOrchestration, which provides runtime (late) binding between application systems. 

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestration – Service Description](../../assets/sd/5_0_0/service-orchestration_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### pull

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationRequest](../data-models/service-orchestration-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/pull HTTP/1.1
Authorization: Bearer <identity-info>

{
   "serviceRequirement": {
      "serviceDefinition": "kelvinInfo",
      "operations": [
         "query-temperature"
      ],
      "versions": [],
      "alivesAt": "2025-10-05T11:35:14Z",
      "metadataRequirements": [],
      "interfaceTemplateNames": [
         "generic_https"
      ],
      "interfaceAddressTypes": [
         "HOSTNAME",
         "IPV4"
      ],
      "interfacePropertyRequirements": [],
      "securityPolicies": [
         "TIME_LIMITED_TOKEN_AUTH"
      ],
      "preferredProviders": []
   },
   "orchestrationFlags": {
      "MATCHMAKING": "true",
      "ALLOW_TRANSLATION": "true",
      "ONLY_PREFERRED": "false",
      "ONLY_EXCLUSIVE": "false",
      "ALLOW_INTERCLOUD": "false",
      "ONLY_INTERCLOUD": "false"
   },
   "qosRequirements": {
      "maxLatencyMs": "10"
   },
   "exclusivityDuration": 600
}
```

The service operation **responds** with the status code `200` if called successfully and with a [ServiceOrchestrationResponse](../data-models/service-orchestration-response.md) JSON encoded body.

```
{
   "results": [
      {
         "serviceInstanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
         "providerName": "TemperatureProvider2",
         "serviceDefinitition": "kelvinInfo",
         "version": "1.0.0",
         "cloudIdentitifer": "LOCAL",
         "aliveUntil": "2028-11-08T10:21:11Z",
         "exclusiveUntil": "2025-10-05T11:35:14Z",
         "metadata": {
            "marginOfError": 0.5
         },
         "interfaces": [
            {
               "templateName": "generic_https",
               "protocol": "https",
               "policy": "TIME_LIMITED_TOKEN_AUTH",
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
         "authorizationTokens": {
            "TIME_LIMITED_TOKEN_AUTH": {
               "query-temperature": {
                  "tokenType": "TIME_LIMITED_TOKEN",
                  "targetType": "SERVICE_DEF",
                  "token": "dsalefb521vdjkdsae633",
                  "expiresAt": "2025-10-05T11:35:14Z"
               }
            }
         }
      }
   ],
   "warnings": [
      "part_time_exclusivity"
   ]
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission, `500` if an unexpected error happens and `503` if an unexpected error happens while communicating with other systems. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "QoS requirements are present, but QoS support is not enabled",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/pull"
}
```

### subscribe

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationSubscriptionRequest](../data-models/service-orchestration-subscription-request.md)
JSON encoded body.

```
POST /serviceorchestration/orchestration/subscribe?trigger=<trigger-value> HTTP/1.1
Authorization: Bearer <identity-info>

{
   "orchestrationRequest": {
      "serviceRequirement": {
         "serviceDefinition": "kelvinInfo",
         "operations": [
            "query-temperature"
         ],
         "versions": [
            
         ],
         "alivesAt": "2025-10-05T11:35:14Z",
         "metadataRequirements": [
            
         ],
         "interfaceTemplateNames": [
            "generic_https"
         ],
         "interfaceAddressTypes": [
            "HOSTNAME",
            "IPV4"
         ],
         "interfacePropertyRequirements": [
            
         ],
         "securityPolicies": [
            "TIME_LIMITED_TOKEN_AUTH"
         ],
         "preferredProviders": [
            
         ]
      },
      "orchestrationFlags": {
         "MATCHMAKING": "true",
         "ALLOW_TRANSLATION": "true",
         "ONLY_PREFERRED": "false",
         "ONLY_EXCLUSIVE": "false",
         "ALLOW_INTERCLOUD": "false",
         "ONLY_INTERCLOUD": "false"
      },
      "qosRequirements": {
         "maxLatencyMs": "10"
      },
      "exclusivityDuration": 600
   },
   "notifyInterface": {
      "protocol": "mqtt",
      "properties": {
         "topic": "arrowhead/orchestration-push"
      }
   },
   "duration": 100000
}
```

The service operation **responds** with the status code `200` if called successfully and an existing subscription was overwritten or `201` if the subscription was newly created. The response also contains a [UUID](../primitives.md#uuid) text body.

```
d4d61873-07db-4e93-a16e-9465852bdabf
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Unsupported notify protocol: CoAP",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/subscribe"
}
```

### unsubscribe

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a subscription [UUID](../primitives.md#uuid) as path parameter.

```
DELETE /serviceorchestration/orchestration/unsubscribe/d4d61873-07db-4e93-a16e-9465852bdabf HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully and an existing subscription was removed and `204` if no matching subscription was found. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid subscription id",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "DELETE /serviceorchestration/orchestration/unsubscribe"
}
```

