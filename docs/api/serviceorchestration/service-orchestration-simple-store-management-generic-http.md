# serviceOrchestrationSimpleStoreManagement IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of serviceOrchestrationSimpleStoreManagement, which enables systems (with operator role or proper permissions) to manage orchestration rules.

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationSimpleStoreManagement – Service Description](../../assets/sd/5_2_0/service-orchestration-simple-store-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationSimpleStoreQueryRequest](../data-models/service-orchestration-simple-store-query-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/simple-store/query HTTP/1.1
Authorization: Bearer <identity-info>

{
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
  "serviceInstanceIds": [],
  "minPriority": 1,
  "maxPriority": 25,
  "createdBy": ""
}
```

The service operation **responds** with the status code `200` if records were successfully queried. The response also contains a [ServiceOrchestrationSimpleStoreListResponse](../data-models/service-orchestration-simple-store-list-response.md) JSON encoded body.

```
{
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
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Minimum priority should not be greater than maximum priority",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/simple-store/query"
}
```

### create

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationSimpleStoreCreateListRequest](../data-models/service-orchestration-simple-store-create-list-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/simple-store/create HTTP/1.1
Authorization: Bearer <identity-info>

{
  "entries": [
    {
      "consumer": "TestConsumerB",
      "serviceInstanceId": "TestProvider|testService|1.0.0",
      "priority": 2
    }
  ]
}
```

The service operation **responds** with the status code `201` if records were successfully created. The response also contains a [ServiceOrchestrationSimpleStoreListResponse](../data-models/service-orchestration-simple-store-list-response.md) JSON encoded body.

```
{
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
      "updatedAt": "2025-11-08T12:00:00Z"
    }
  ],
  "count": 1
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Priority is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/simple-store/create"
}
```

###  modify-priorities

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationSimpleStorePriorityMap](../data-models/service-orchestration-simple-store-priority-map.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/simple-store/modify-priorities HTTP/1.1
Authorization: Bearer <identity-info>

{
  "0308e42e-af59-4206-8513-3414bb2dbc60": 1,
  "ace0f20b-b3aa-4935-adc7-d5dc58d4dc99": 2
}
```

The service operation **responds** with the status code `200` if records were successfully modified. The response also contains a [ServiceOrchestrationSimpleStoreListResponse](../data-models/service-orchestration-simple-store-list-response.md) JSON encoded body.

```
{
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
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Priority map contains null value",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/simple-store/modify-priorities"
}
```

### remove


The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a query parameter _uuids_, which is a List<[UUID](../primitives.md#uuid)>. It contains the identifiers of the store entries to remove.

```
DELETE /serviceorchestration/orchestration/mgmt/simple-store/query?uuids=d2fefc6a-f563-40a2-9ce4-3512c2887755&ids=a44ab333-cfb5-420b-a7cf-b327904e243b HTTP/1.1
Authorization: Bearer <identity-info>
```
The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid UUID: notanid",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "DELETE /serviceorchestration/orchestration/mgmt/simple-store/remove"
}
```