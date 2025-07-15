# serviceOrchestrationLockManagement IDD (dynamic strategy)
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of serviceOrchestrationLockManagement, which enables systems to manage orchestration lock records in bulk. It’s implemented using protocol, encoding as stated in the following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationLockManagement – Service Description](../../assets/sd/5_0_0/service-orchestration-lock-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### create

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationLockListRequest](../data-models/service-orchestration-lock-list-request.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/management/lock/create

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
      "locks": [
         {
            "serviceInstanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
            "owner": "TemperatureManager",
            "expiresAt": "2025-11-08T11:24:43Z"
         }
      ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if lock records were successfully created. The response template payload is a [ServiceOrchestrationLockListResponse](../data-models/service-orchestration-lock-list-response.md).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "TemperatureManager",
   "payload": {
      "entries": [
         {
            "id": 5,
            "orchestrationJobId": "091cc9c3-e704-4d0a-8d70-96fb2c11da55",
            "serviceInstanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
            "owner": "TemperatureManager",
            "expiresAt": "2025-11-08T11:24:43Z",
            "temporary": false
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
   "receiver": "TemperatureManager",
   "payload": {
      "errorMessage": "Owner is missing",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/lock/create"
   }
}
```

### query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationLockQueryRequest](../data-models/service-orchestration-lock-query-request.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/management/lock/query

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
         "sortField": "owner"
      },
      "ids": [
         5
      ],
      "orchestrationJobIds": [
         "091cc9c3-e704-4d0a-8d70-96fb2c11da55"
      ],
      "serviceInstanceIds": [
         "TemperatureProvider2|kelvinInfo|1.0.0"
      ],
      "owners": [
         "TemperatureManager"
      ],
      "expiresBefore": "2025-11-08T12:00:00Z",
      "expiresAfter": "2025-11-08T11:00:00Z"
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if lock records were successfully queried. The response template payload is a [ServiceOrchestrationLockListResponse](../data-models/service-orchestration-lock-list-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TemperatureManager",
   "payload": {
      "entries": [
         {
            "id": 5,
            "orchestrationJobId": "091cc9c3-e704-4d0a-8d70-96fb2c11da55",
            "serviceInstanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
            "owner": "TemperatureManager",
            "expiresAt": "2025-11-08T11:24:43Z",
            "temporary": false
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
   "receiver": "TemperatureManager",
   "payload": {
      "errorMessage": "Invalid orchestration job id: abc123",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/lock/query"
   }
}
```

### remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[ServiceInstanceID](../primitives.md#serviceinstanceid)>. An `owner` parameter also has to be provided with a [SystemName](../primitives.md#systemname) value.

```
Topic: arrowhead/serviceorchestration/orchestration/management/lock/remove

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "params": {
      "owner": "TemperatureManager"
   },
   "payload": [
        "TemperatureProvider2|kelvinInfo|1.0.0"
   ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. 

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "TemperatureManager"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TemperatureManager",
   "payload": {
      "errorMessage": "Owner is missing",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/serviceorchestration/orchestration/management/lock/remove"
   }
}
```