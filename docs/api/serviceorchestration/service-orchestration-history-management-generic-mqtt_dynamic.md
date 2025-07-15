# serviceOrchestrationHistoryManagement IDD (dynamic strategy)
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of serviceOrchestrationHistoryManagement, which enables systems to gather information about the performed orchestration processes. It’s implemented using protocol, encoding as stated in the following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationHistoryManagement – Service Description](../../assets/sd/5_0_0/service-orchestration-history-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceOrchestrationHistoryQueryRequest](../data-models/service-orchestration-history-query-request.md). 

```
Topic: arrowhead/serviceorchestration/orchestration/management/history/query

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
      "ids": [
         "5cf0f11a-6bea-46c7-8550-403c96d27bbc"
      ],
      "statuses": [
         "PENDING",
         "IN_PROGRESS"
      ],
      "type": "PUSH",
      "requesterSystems": [
         "TemperatureManager"
      ],
      "targetSystems": [
         "TemperatureConsumer"
      ],
      "serviceDefinitions": [
         "kelvinInfo"
      ],
      "subscriptionIds": [
         "3a07c069-e070-4831-a5ad-5307c112354f"
      ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if job records were successfully queried. The response template payload is a [ServiceOrchestrationHistoryResponse](../data-models/service-orchestration-history-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TemperatureManager",
   "payload": {
      "entries": [
         {
            "id": "5cf0f11a-6bea-46c7-8550-403c96d27bbc",
            "status": "IN_PROGRESS",
            "type": "PUSH",
            "requesterSystem": "TemperatureManager",
            "targetSystem": "TemperatureConsumer",
            "serviceDefinition": "kelvinInfo",
            "subscriptionId": "3a07c069-e070-4831-a5ad-5307c112354f",
            "message": "",
            "createdAt": "2025-11-08T12:00:00Z",
            "startedAt": "2025-11-08T12:01:00Z",
            "finishedAt": ""
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
      "origin": "arrowhead/serviceorchestration/orchestration/management/history/query"
   }
}
```