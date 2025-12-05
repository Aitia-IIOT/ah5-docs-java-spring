# serviceOrchestrationSimpleStoreManagement IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-http-template.md) and [generic_mqtts](../communication-profiles/generic-https-template.md) service interface of serviceOrchestrationSimpleStoreManagement, which enables systems (with operator role or proper permissions) to manage orchestration rules.

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationSimpleStoreManagement – Service Description](../../assets/sd/5_2_0/service-orchestration-simple-store-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationSimpleStoreQueryRequest](../data-models/service-orchestration-simple-store-query-request.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/management/store/query

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
      "pagination": {
         "page": 0,
         "size": 10,
         "direction": "ASC",
         "sortField": "createdAt"
      },
      "ids": "341f7626-bdee-4ec1-8cb9-23f88e04e374",
      "consumerNames": [
         "TestConsumerA",
         "TestConsumerB"
      ],
      "serviceDefinitions": [
         "testService"
      ],
      "serviceInstanceIds": [
         
      ],
      "minPriority": 1,
      "maxPriority": 25,
      "createdBy": ""
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if records were successfully queried. The response template payload is a [ServiceOrchestrationSimpleStoreListResponse](../data-models/service-orchestration-simple-store-list-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "Sysop",
   "payload": {
      "entries": [
         {
            "id": "0308e42e-af59-4206-8513-3414bb2dbc60",
            "consumer": "TestConsumerB",
            "serviceDefinition": "testService",
            "serviceInstanceId": "TestProvider|testService|1.0.0",
            "priority": 2,
            "createdBy": "Sysop",
            "updatedBy": "Sysop",
            "createdAt": "2025-11-08T12:00:00Z",
            "updatedAt": "2025-11-08T15:14:00Z"
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
   "receiver": "Sysop",
   "payload": {
      "errorMessage": "Minimum priority should not be greater than maximum priority",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/store/query"
   }
}
```

### create

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationSimpleStoreCreateListRequest](../data-models/service-orchestration-simple-store-create-list-request.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/management/store/create

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
      "entries": [
        {
          "consumer": "TestConsumerB",
          "serviceInstanceId": "TestProvider|testService|1.0.0",
          "priority": 2
        }
      ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if records were successfully created. The response template payload is a [ServiceOrchestrationSimpleStoreListResponse](../data-models/service-orchestration-simple-store-list-response.md).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "Sysop",
   "payload": {
      "entries": [
         {
            "id": "0308e42e-af59-4206-8513-3414bb2dbc60",
            "consumer": "TestConsumerB",
            "serviceDefinition": "testService",
            "serviceInstanceId": "TestProvider|testService|1.0.0",
            "priority": 2,
            "createdBy": "Sysop",
            "updatedBy": "Sysop",
            "createdAt": "2025-11-08T12:00:00Z",
            "updatedAt": "2025-11-08T15:14:00Z"
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
   "receiver": "Sysop",
   "payload": {
      "errorMessage": "Priority is missing",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/store/create"
   }
}
```

###  modify-priorities

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationSimpleStorePriorityMap](../data-models/service-orchestration-simple-store-priority-map.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/management/store/modify-priorities

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
      "0308e42e-af59-4206-8513-3414bb2dbc60": 1,
      "ace0f20b-b3aa-4935-adc7-d5dc58d4dc99": 2
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if records were successfully modified. The response template payload is a [ServiceOrchestrationSimpleStoreListResponse](../data-models/service-orchestration-simple-store-list-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "Sysop",
   "payload": {
      "entries": [
         {
            "id": "0308e42e-af59-4206-8513-3414bb2dbc60",
            "consumer": "TestConsumerB",
            "serviceDefinition": "testService",
            "serviceInstanceId": "TestProvider|testService|1.0.0",
            "priority": 1,
            "createdBy": "Sysop",
            "updatedBy": "Sysop",
            "createdAt": "2025-11-08T12:00:00Z",
            "updatedAt": "2025-11-08T12:00:00Z"
          },
          {
            "id": "ace0f20b-b3aa-4935-adc7-d5dc58d4dc99",
            "consumer": "TestConsumerB",
            "serviceDefinition": "testService",
            "serviceInstanceId": "OtherProvider|testService|1.0.0",
            "priority": 2,
            "createdBy": "Sysop",
            "updatedBy": "Sysop",
            "createdAt": "2025-11-08T12:00:00Z",
            "updatedAt": "2025-11-08T12:00:00Z"
          }
      ],
      "count": 2
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "Sysop",
   "payload": {
      "errorMessage": "Priority map contains null value",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/store/modify-priorities"
   }
}
```

### remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[UUID](../primitives.md#uuid)>. It contains the identifiers of the store entries to remove.

```
Topic: arrowhead/serviceorchestration/orchestration/management/store/remove

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": [
      "0308e42e-af59-4206-8513-3414bb2dbc60",
      "ace0f20b-b3aa-4935-adc7-d5dc58d4dc99"
   ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is empty. 

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "Sysop",
   "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "Sysop",
   "payload": {
      "errorMessage": "Invalid UUID: notanid",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/store/remove"
   }
}
```