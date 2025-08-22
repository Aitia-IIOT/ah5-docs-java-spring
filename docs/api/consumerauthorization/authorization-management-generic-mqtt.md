# authorizationManagement IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of authorizationManagement, which enables systems (with operator role or proper permissions) to handle (grant, revoke, query, check) authorization policies in bulk. An example of this interaction is when an operator uses the Management Tool to set up authorization policies manually before the related systems even register themselves. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

Hereby the **Interface Design Description** (IDD) is provided to the [authorizationManagement – Service Description](../../assets/sd/5_0_0/authorization-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### grant-policies

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationMgmtGrantListRequest](../data-models/authorization-mgmt-grant-list-request.md).


```
Topic: arrowhead/consumer-authorization/authorization/management/grant-policies

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "list": [
       {
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
         }
       }
     ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and the policy instance is already existing or `201` if the entity was newly created. The response template payload is an [AuthorizationPolicyListResponse](../data-models/authorization-policy-list-response.md).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "entries": [
       {
         "instanceId": "MGMT|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo",
         "level": "MGMT",
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
         "createdBy": "Sysop",
         "createdAt": "2025-06-23T08:35:43.217717900Z"
       }
     ],
     "count": 1
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
     "origin": "arrowhead/consumer-authorization/authorization/management/grant-policies"
   }
}
```

### revoke-policies

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[AuthorizationPolicyInstanceID](../primitives.md#authorizationpolicyinstanceid)>.

```
Topic: arrowhead/consumer-authorization/authorization/management/revoke-policies

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": [ 
      "MGMT|LOCAL|TemperatureProvider|SERVICE_DEF|celsiusInfo"
   ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. 

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
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "errorMessage": "Instance id list is missing",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/consumer-authorization/authorization/management/revoke-policies"
   }
}
```

### query-policies

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationQueryRequest](../data-models/authorization-query-request.md).


```
Topic: arrowhead/consumer-authorization/authorization/management/query-policies

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "pagination": {
       "page": 0,
       "size": 10
     },
     "level": "MGMT",
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
         "instanceId": "MGMT|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo",
         "level": "MGMT",
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
         "createdBy": "Sysop",
         "createdAt": "2025-06-23T08:35:43Z"
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
     "errorMessage": "Level is missing",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/consumer-authorization/authorization/management/query-policies"
   }
}
```

### check-policies

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationMgmtVerifyListRequest](../data-models/authorization-mgmt-verify-list-request.md).

```
Topic: arrowhead/consumer-authorization/authorization/management/check-policies

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "list": [
       {
         "provider": "TemperatureProvider2",
         "consumer": "TemperatureManager",
         "targetType": "SERVICE_DEF",
         "target": "kelvinInfo",
         "scope": "config"
       }
     ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is an [AuthorizationMgmtVerifyListResponse](../data-models/authorization-mgmt-verify-list-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "entries": [
       {
         "provider": "TemperatureProvider2",
         "consumer": "TemperatureManager",
         "cloud": "LOCAL",
         "targetType": "SERVICE_DEF",
         "target": "kelvinInfo",
         "scope": "config",
         "granted": true
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
     "errorMessage": "Provider is missing",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/consumer-authorization/authorization/management/check-policies"
   }
}
```