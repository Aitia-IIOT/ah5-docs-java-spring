# blacklistManagement IDD
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of blacklistManagement which enables systems (with operator role or proper permissions) to handle (query, create, remove) blacklist entries.
This service interface is implemented using protocol, encoding as stated in the following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [blacklistManagement - Service Description](../../assets/sd/5_0_0/blacklistManagement_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [BlacklistQueryRequest](../data-models/blacklist-query-request.md) JSON encoded body. Note that if _alivesAt_ is set, inactive records will not be returned.

```
POST /blacklist/mgmt/query HTTP/1.1
Authorization: Bearer <identity-info>

{
  "pagination": {
    "page": 0,
    "size": 5,
    "direction": "ASC",
    "sortField": "createdAt"
  },
  "systemNames": [
  ],
  "mode": "ACTIVES",
  "issuers": [
    "Sysop"
  ],
  "revokers": [
  ],
  "reason": "temporary_ban",
  "alivesAt": "2025-06-05T23:59:59Z"
}
```
The service operation **responds** with the status code `200` if called successfully and with a [BlacklistEntryListResponse](../data-models/blacklist-entry-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "systemName": "AlertConsumer1",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:07Z",
      "updatedAt": "2025-06-05T13:43:07Z",
      "reason": "temporary_ban",
      "expiresAt": "2025-12-31T23:59:59Z",
      "active": true
    },
    {
      "systemName": "AlertConsumer2",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:07Z",
      "updatedAt": "2025-06-05T13:43:07Z",
      "reason": "temporary_ban",
      "expiresAt": "2025-12-31T23:59:59Z",
      "active": true
    }
  ],
  "count": 2
}
```
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Mode is invalid. Possible values: ALL, ACTIVES, INACTIVES",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /blacklist/mgmt/query"
}
```

### create

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [BlacklistCreateListRequest](../data-models/blacklist-create-list-request.md) JSON encoded body.

```
POST /blacklist/mgmt/create HTTP/1.1
Authorization: Bearer <identity-info>

{
  "entities": [
    {
      "systemName": "TemperatureProvider1",
      "expiresAt": "",
      "reason": "This provider is broken and sends too many false alarms. Should be fixed."
    },
    {
      "systemName": "AlertConsumer1",
      "expiresAt": "2025-12-31T23:59:59Z",
      "reason": "temporary_ban"
    },
    {
      "systemName": "AlertConsumer2",
      "expiresAt": "2025-12-31T23:59:59Z",
      "reason": "temporary_ban"
    }
  ]
}
```
The service operation **responds** with the status code `201` if the blacklist entries were successfully created. The response also contains a [BlacklistEntryListResponse](../data-models/blacklist-entry-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "systemName": "TemperatureProvider1",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:06.568772600Z",
      "updatedAt": "2025-06-05T13:43:06.568772600Z",
      "reason": "This provider is broken and sends too many false alarms. Should be fixed.",
      "active": true
    },
    {
      "systemName": "AlertConsumer1",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:06.705704400Z",
      "updatedAt": "2025-06-05T13:43:06.705704400Z",
      "reason": "temporary_ban",
      "expiresAt": "2025-12-31T23:59:59Z",
      "active": true
    },
    {
      "systemName": "AlertConsumer2",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:06.707704700Z",
      "updatedAt": "2025-06-05T13:43:06.707704700Z",
      "reason": "temporary_ban",
      "expiresAt": "2025-12-31T23:59:59Z",
      "active": true
    }
  ],
  "count": 3
}
```
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "You cannot blacklist a system without specifying the reason",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /blacklist/mgmt/create"
}
```

### remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a query parameter _names_, which is a List<[SystemName](../primitives.md#systemname)>. It contains the names of the systems to remove from the blacklist. This means that their active entries will be inactivated.

```
DELETE /blacklist/mgmt/remove?names=AlertConsumer1&names=AlertConsumer2 HTTP/1.1
Authorization: Bearer <identity-info>
```
The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "TemperatureProvider1 system is blacklisted",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN"
}
```