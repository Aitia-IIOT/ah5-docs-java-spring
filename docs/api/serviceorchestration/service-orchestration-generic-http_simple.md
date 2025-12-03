# serviceOrchestration IDD (simple strategy)
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
      "preferredProviders": []
   },
   "orchestrationFlags": {
      "MATCHMAKING": "true",
      "ONLY_PREFERRED": "false"
   }
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
         "cloudIdentitifer": "LOCAL"
      }
   ],
   "warnings": []
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "The specified service definition name does not match the naming convention: exampleservicename",
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
         "preferredProviders": []
      },
      "orchestrationFlags": {
         "MATCHMAKING": "true",
         "ONLY_PREFERRED": "false"
      }      
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

