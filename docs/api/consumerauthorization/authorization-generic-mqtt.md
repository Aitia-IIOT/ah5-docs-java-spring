# authorization IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of authorization, which enables service consumption permission validations
for both providers and consumers. Additionally, providers can lookup, grant and revoke those permissions. An example of this interaction when a provider system creates authorization policies about its offered service. An
other example when a consumer can check whether a service is allowed to use before trying an actual service consumption. Event notification permission is also handled by this service in an event publisher/subscriber
scenario. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry. 

The interfaces are implemented using protocol, encoding as stated in the following tables:

## Interface Description

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

Hereby the **Interface Design Description** (IDD) is provided to the [authorization – Service Description](../../assets/sd/5_0_0/authorization_sd.pdf). For further details about how this service is meant to be used, please consult that document.

### grant

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationGrantRequest](../data-models/authorization-grant-request.md).


```
Topic: arrowhead/consumer-authorization/authorization/grant

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "targetType": "SERVICE_DEF",
     "target": "kelvinInfo",
     "description": "query for everyone, config for TemperatureManager only",
     "defaultPolicy": {
       "policyType": "ALL"
     },
     "scopedPolicies": {
        "config": {
        "policyType": "WHITELIST",
        "policyList": [
          "TemperatureManager"
        ]
      }
    }
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and the policy instance is already existing or `201` if the entity was newly created. The response template payload is an [AuthorizationPolicyResponse](../data-models/authorization-policy-response.md).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "instanceId": "PR|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo",
     "level": "PROVIDER",
     "cloud": "LOCAL",
     "provider": "TemperatureProvider2",
     "targetType": "SERVICE_DEF",
     "target": "kelvinInfo",
     "description": "query for everyone, config for TemperatureManager only",
     "defaultPolicy": {
       "policyType": "ALL"
     },
     "scopedPolicies": {
       "config": {
         "policyType": "WHITELIST",
         "policyList": [
           "TemperatureManager"
         ]
       }
     },
     "createdBy": "TemperatureProvider2",
     "createdAt": "2025-06-18T13:51:19.727425900Z"
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
     "errorMessage": "Target is missing"",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/consumer-authorization/authorization/grant"
   }
}
```

### revoke

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationPolicyInstanceID](../primitives.md#authorizationpolicyinstanceid).

```
Topic: arrowhead/consumer-authorization/authorization/revoke 

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": "PR|LOCAL|TemperatureProvider|SERVICE_DEF|celsiusInfo"
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and an existing policy instance entity was removed and `204` if no matching entity was found. 

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
   "status": 403,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "errorMessage": "Revoking other systems' policy is forbidden",
     "errorCode": 403,
     "exceptionType": "FORBIDDEN",
     "origin": "arrowhead/consumer-authorization/authorization/revoke "
   }
}
```

### lookup

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationLookupRequest](../data-models/authorization-lookup-request.md).


```
Topic: arrowhead/consumer-authorization/authorization/lookup

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "instanceIds": [
     ],
     "cloudIdentifiers": [
     ],
     "targetNames": [
       "kelvinInfo"
     ],
	 "targetType": "SERVICE_DEF"
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is an [AuthorizationPolicyListResponse](../data-models/authorization-policy-list-response.md).

```

{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "entries": [
       {
         "instanceId": "PR|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo",
         "level": "PROVIDER",
         "cloud": "LOCAL",
         "provider": "TemperatureProvider2",
         "targetType": "SERVICE_DEF",
         "target": "kelvinInfo",
         "description": "query for everyone, config for TemperatureManager only",
         "defaultPolicy": {
           "policyType": "ALL"
         },
         "scopedPolicies": {
           "config": {
             "policyType": "WHITELIST",
             "policyList": [
               "TemperatureManager"
             ]
           }
         },
         "createdBy": "TemperatureProvider2",
         "createdAt": "2025-06-18T13:51:20Z"
       }
     ],
     "count": 1
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
     "errorMessage": "One of the following filters must be used: 'instanceIds', 'targetNames', 'cloudIdentifiers'",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/consumer-authorization/authorization/lookup"
   }
}
```

### verify

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationVerifyRequest](../data-models/authorization-verify-request.md).

```
Topic: arrowhead/consumer-authorization/authorization/verify

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "provider": "TemperatureProvider2",
     "consumer": "TemperatureManager",
     "targetType": "SERVICE_DEF",
     "target": "kelvinInfo",
     "scope": "config"
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [Boolean](../primitives.md#boolean).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": true
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 403,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "errorMessage": "Only the related provider or consumer can use this operation",
     "errorCode": 403,
     "exceptionType": "FORBIDDEN",
     "origin": "arrowhead/consumer-authorization/authorization/verify"
  }
}
```