# serviceOrchestrationLockManagement IDD (dynamic strategy)
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of serviceOrchestrationLockManagement, which enables systems to manage orchestration lock records in bulk. It’s implemented using protocol, encoding as stated in the following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [serviceOrchestrationLockManagement – Service Description](../../assets/sd/5_0_0/service-orchestration-lock-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### create

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationLockListRequest](../data-models/service-orchestration-lock-list-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/lock/create HTTP/1.1
Authorization: Bearer <identity-info>

{
  "locks": [
    {
      "serviceInstanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
      "owner": "TemperatureManager",
      "expiresAt": "2025-11-08T11:24:43Z"
    }
  ]
}
```

The service operation **responds** with the status code `201` if locks were successfully created. The response also contains a [ServiceOrchestrationLockListResponse](../data-models/service-orchestration-lock-list-response.md) JSON encoded body.

```
{
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
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Owner is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/lock/create"
}
```

### query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceOrchestrationLockQueryRequest](../data-models/service-orchestration-lock-query-request.md) JSON encoded body.

```
POST /serviceorchestration/orchestration/mgmt/lock/query HTTP/1.1
Authorization: Bearer <identity-info>

{
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
```

The service operation **responds** with the status code `200` if locks were successfully queried. The response also contains a [ServiceOrchestrationLockListResponse](../data-models/service-orchestration-lock-list-response.md) JSON encoded body.

```
{
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
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid orchestration job id: abc123",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/lock/query"
}
```

### remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http), an owner [SystemName](../primitives.md#systemname) path variable and a query parameter _instanceIds_, which is a List<[ServiceInstanceID](../primitives.md#serviceinstanceid)>.

```
DELETE /serviceorchestration/orchestration/mgmt/lock/remove/TemperatureManager?instanceIds=TemperatureProvider2%7CkelvinInfo%7C1.0.0 HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Owner is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceorchestration/orchestration/mgmt/lock/remove"
}
```