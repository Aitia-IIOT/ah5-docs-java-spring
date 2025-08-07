# blacklistDiscovery IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of blacklistDiscovery which enables both Application and Core/Support systems to query the blacklist entries in force that apply to them, or check if a system is blacklisted. Note that a record is in force if it is `ACTIVE` _and_ not expired.

Hereby the **Interface Design Description** (IDD) is provided to the [blacklistDiscovery - Service Description](../../assets/sd/5_0_0/blacklistDiscovery_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### lookup

The requester can lookup for relevant entries that apply to them and are in force. The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http). The requester name will be identified during authentication.

```
GET /blacklist/lookup HTTP/1.1
Authorization: Bearer <identity-info>
```
The service operation **responds** with the status code `200` if called successfully and with a [BlacklistEntryListResponse](../data-models/blacklist-entry-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "systemName": "TemperatureProvider1",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T14:15:02Z",
      "updatedAt": "2025-06-05T14:15:02Z",
      "reason": "Needs further repair.",
      "active": true
    }
  ],
  "count": 1
}
```
The **error codes** are `401` if the requester authentication was unsuccessful or `500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid authorization header",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```

### check

The requester can check whether a system is on the blacklist. The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [SystemName](../primitives.md#systemname) as path parameter, which identifies the name of the system to check.

```
GET /blacklist/check/AlertConsumer1 HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully and a [Boolean](../primitives.md#boolean) value which idicates if the system is blacklisted or not.

```
false
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "The specified system name does not match the naming convention: AlertCon$umer1",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "GET /blacklist/check/AlertCon$umer1"
}
```