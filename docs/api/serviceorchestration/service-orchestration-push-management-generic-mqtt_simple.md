# serviceOrchestrationPushManagement IDD (simple strategy)
**generic_mqtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of serviceOrchestrationPushManagement, which enables systems to manage data and activities related to the push type of service orchestration process in bulk.

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationPushManagement – Service Description](../../assets/sd/5_0_0/service-orchestration-push-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### subscribe

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationSubscriberListRequest](../data-models/service-orchestration-subscriber-list-request.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/management/push/subscribe

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
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
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if subscriptions were successfully created. The response template payload is a [ServiceOrchestrationSubscriptionListResponse](../data-models/service-orchestration-subscription-list-response.md).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "TemperatureSensorManager",
   "payload": {
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
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TemperatureSensorManager",
   "payload": {
      "errorMessage": "Subscription request list is empty",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/push/subscribe"
   }
}
```

### unsubscribe

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[UUID](../primitives.md#uuid)>. It contains the identifiers of the subscription records to remove.

```
Topic: arrowhead/serviceorchestration/orchestration/management/push/unsubscribe

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": [
      "d2fefc6a-f563-40a2-9ce4-3512c2887755",
      "a44ab333-cfb5-420b-a7cf-b327904e243b"
   ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is empty. 

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TemperatureSensorManager",
   "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 403,
   "traceId": "<trace-id>",
   "receiver": "TemperatureSensorManager",
   "payload": {
      "errorMessage": "a44ab333-cfb5-420b-a7cf-b327904e243b is not owned by the requester",
      "errorCode": 403,
      "exceptionType": "FORBIDDEN",
      "origin": "arrowhead/serviceorchestration/orchestration/management/push/unsubscribe"
   }
}
```

### trigger 

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationPushTriggerRequest](../data-models/service-orchestration-push-trigger-request.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/management/push/trigger

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
      "targetSystems": [
         "TemperatureConsumer"
      ],
      "subscriptionIds": [
         
      ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if orchestration jobs were successfully created. The response template payload is a [ServiceOrchestrationJobListResponse](../data-models/service-orchestration-job-list-response.md).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "TemperatureSensorManager",
   "payload": {
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
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TemperatureSensorManager",
   "payload": {
      "errorMessage": "Invalid subscription id: a44ab333-cfb5-420b-a7cf-b327904e243b",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/push/trigger"
   }
}
```

### query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationSubscriptionQueryRequest](../data-models/service-orchestration-subscription-query-request.md).

```
Topic: arrowhead/serviceorchestration/orchestration/management/push/query

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
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
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if subscriptions were successfully queried. The response template payload is a [ServiceOrchestrationSubscriptionListResponse](../data-models/service-orchestration-subscription-list-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TemperatureSensorManager",
   "payload": {
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
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TemperatureSensorManager",
   "payload": {
      "errorMessage": "Owner system list contains empty element",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/push/query"
   }
}
```