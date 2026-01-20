# serviceOrchestrationPushManagement IDD (simple strategy)
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of serviceOrchestrationPushManagement, which enables systems to manage data and activities related to the push type of service orchestration process in bulk.

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationPushManagement – Service Description](../../assets/sd/5_0_0/service-orchestration-push-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### subscribe

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationSubscriberListRequest](../data-models/service-orchestration-subscriber-list-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/push/subscribe HTTP/1.1
Authorization: Bearer <identity-info>

{
   "subscriptions": [
      {
         "targetSystemName": "TemperatureConsumer",
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
   ]
}
```

The service operation **responds** with the status code `201` if subscriptions were successfully created. The response also contains a [ServiceOrchestrationSubscriptionListResponse](../data-models/service-orchestration-subscription-list-response.md) JSON encoded body.

```
{
   "entries": [
      {
         "id": "d2fefc6a-f563-40a2-9ce4-3512c2887755",
         "ownerSystemName": "TemperatureSensorManager",
         "targetSystemName": "TemperatureConsumer",
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
         "expiredAt": "2025-10-08T11:35:14Z",
         "createdAt": "2025-10-05T11:30:14Z"
      }
   ],
   "count": 1
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Subscription request list is empty",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/push/subscribe"
}
```
### unsubscribe

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a query parameter _ids_, which is a List<[UUID](../primitives.md#uuid)>. It contains the identifiers of the subscription records to remove.

```
DELETE /serviceorchestration/orchestration/mgmt/push/unsubscribe?ids=d2fefc6a-f563-40a2-9ce4-3512c2887755&ids=a44ab333-cfb5-420b-a7cf-b327904e243b HTTP/1.1
Authorization: Bearer <identity-info>
```
The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "a44ab333-cfb5-420b-a7cf-b327904e243b is not owned by the requester",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN",
  "origin": "DELETE /serviceorchestration/orchestration/mgmt/push/unsubscribe"
}
```

### trigger

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationPushTriggerRequest](../data-models/service-orchestration-push-trigger-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/push/trigger HTTP/1.1
Authorization: Bearer <identity-info>

{
   "targetSystems": [
      "TemperatureConsumer"
   ],
   "subscriptionIds": [
      
   ]
}
```

The service operation **responds** with the status code `201` if orchestration jobs were successfully created. The response also contains a [ServiceOrchestrationJobListResponse](../data-models/service-orchestration-job-list-response.md) JSON encoded body.

```
{
   "jobs": [
      {
         "id": "1fac8a1c-aa4b-456e-b501-01289035fcc6",
         "status": "IN_PROGRESS",
         "type": "PUSH",
         "requesterSystem": "TemperatureSensorManager",
         "targetSystem": "TemperatureConsumer",
         "serviceDefinition": "kelvinInfo",
         "subscriptionId": "d2fefc6a-f563-40a2-9ce4-3512c2887755",
         "message": "",
         "createdAt": "2025-10-05T11:30:14Z",
         "startedAt": "2025-10-05T11:30:17Z",
         "finishedAt": ""
      }
   ]
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid subscription id: a44ab333-cfb5-420b-a7cf-b327904e243b",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/push/trigger"
}
```

### query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationSubscriptionQueryRequest](../data-models/service-orchestration-subscription-query-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/push/query HTTP/1.1
Authorization: Bearer <identity-info>

{
   "pagination": {
      "page": 0,
      "size": 20,
      "direction": "ASC",
      "sortField": "createdAt"
   },
   "ownerSystems": [
      "TemperatureSensorManager"
   ],
   "targetSystems": [
      "TemperatureConsumer"
   ],
   "serviceDefinitions": [
      "kelvinInfo"
   ]
}
```

The service operation **responds** with the status code `200` if subscriptions were successfully queried. The response also contains a [ServiceOrchestrationSubscriptionListResponse](../data-models/service-orchestration-subscription-list-response.md) JSON encoded body.

```
{
   "entries": [
      {
         "id": "d2fefc6a-f563-40a2-9ce4-3512c2887755",
         "ownerSystemName": "TemperatureSensorManager",
         "targetSystemName": "TemperatureConsumer",
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
         "expiredAt": "2025-10-08T11:35:14Z",
         "createdAt": "2025-10-05T11:30:14Z"
      }
   ],
   "count": 1
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Owner system list contains empty element",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/push/query"
}
```