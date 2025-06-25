# serviceOrchestrationPushManagement IDD (dynamic strategy)
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of serviceOrchestrationPushManagement, which enables systems to manage data and activities related to the push type of service orchestration.
process in bulk. It’s implemented using protocol, encoding as stated in the following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationPushManagement – Service Description](../../assets/sd/5_0_0/service-orchestration-push-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### subscribe

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationSubscriberListRequest](../data-models/service-orchestration-subscriber-list-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/push/subscribe HTTP/1.1
Authorization: Bearer <identity-info>

{
   "subscriptions":[
      {
         "targetSystemName":"TemperatureConsumer",
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
               "maxLatencyMs":"10"
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
   ]
}
```

The service operation **responds** with the status code `201` if subscriptions were successfully created. he response also contains a [ServiceOrchestrationSubscriptionListResponse](../data-models/service-orchestration-subscription-list-response.md) JSON encoded body.

```
{
   "entries":[
      {
         "id":"d2fefc6a-f563-40a2-9ce4-3512c2887755",
         "ownerSystemName":"TemperatureSensorManager",
         "targetSystemName":"TemperatureConsumer",
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
               "maxLatencyMs":"10"
            },
            "exclusivityDuration":600
         },
         "notifyInterface":{
            "protocol":"mqtt",
            "properties":{
               "topic":"arrowhead/orchestration-push"
            }
         },
         "expiredAt":"2025-10-08T11:35:14Z",
         "createdAt":"2025-10-05T11:30:14Z"
      }
   ],
   "count":1
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Subscription request list is empty.",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/push/subscribe"
}
```