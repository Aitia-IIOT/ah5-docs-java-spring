# serviceOrchestrationHistoryManagement IDD (all strategy)
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of serviceOrchestrationHistoryManagement, which enables systems to gather information about the performed orchestration processes.

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationHistoryManagement – Service Description](../../assets/sd/5_0_0/service-orchestration-history-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationHistoryQueryRequest](../data-models/service-orchestration-history-query-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/history/query HTTP/1.1
Authorization: Bearer <identity-info>

{
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
    "PENDING", "IN_PROGRESS"
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

```

The service operation **responds** with the status code `200` if jobs were successfully queried. The response also contains a [ServiceOrchestrationHistoryResponse](../data-models/service-orchestration-history-response.md) JSON encoded body.

```
{
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
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid orchestration job id: abc123",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/history/query"
}
```