# generalManagement IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of general-management which allows (with operator role or proper permissions)
to get some information about the hosting system’s behavior, such as log entries and configuration settings.

Hereby the **Interface Design Description** (IDD) is provided to the [generalManagement – Service Description](../../assets/sd/5_0_0/general-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### get-log

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [LogRequest](../data-models/log-request.md) JSON encoded body.

```
POST /<system-specific-path>/general/mgmt/logs HTTP/1.1
Authorization: Bearer <identity-info>

{
  "pagination": {
    "page": 0,
    "size": 5,
    "direction": "ASC",
    "sortField": "entryDate"
  },
  "from": "2025-03-07T00:00:00Z",
  "to": "2025-03-08T00:00:00Z",
  "severity": "INFO"
}
```

The service operation **responds** with the status code `200` if called successfully. The response also contains an
[LogResponse](../data-models/log-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "logId": "1d61efc1-fb39-11ef-b55e-2cf05d74cdac",
      "entryDate": "2025-03-07T09:46:59.203Z",
      "logger": "eu.arrowhead.authentication.AuthenticationMain",
      "severity": "INFO",
      "message": "Starting AuthenticationMain using Java 21.0.5 with PID 13072 (omitted)"
    },
    {
      "logId": "1d9b9d62-fb39-11ef-b55e-2cf05d74cdac",
      "entryDate": "2025-03-07T09:46:59.212Z",
      "logger": "eu.arrowhead.authentication.AuthenticationMain",
      "severity": "INFO",
      "message": "No active profile set, falling back to 1 default profile: \"default\""
    },
    {
      "logId": "1e2d2f03-fb39-11ef-b55e-2cf05d74cdac",
      "entryDate": "2025-03-07T09:47:00.973Z",
      "logger": "eu.arrowhead.common.http.filter.authorization.ManagementServiceFilter",
      "severity": "INFO",
      "message": "ManagementServiceFilter is active"
    },
    {
      "logId": "1e4cec04-fb39-11ef-b55e-2cf05d74cdac",
      "entryDate": "2025-03-07T09:47:01.058Z",
      "logger": "eu.arrowhead.authentication.http.filter.InternalAuthenticationFilter",
      "severity": "INFO",
      "message": "InternalAuthenticationFilter is active"
    },
    {
      "logId": "1f529c35-fb39-11ef-b55e-2cf05d74cdac",
      "entryDate": "2025-03-07T09:47:02.897Z",
      "logger": "eu.arrowhead.authentication.quartz.CleanerConfig$$SpringCGLIB$$0",
      "severity": "INFO",
      "message": "Cleaner job is initialized."
    }
  ],
  "count": 26
}

```
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid time interval",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /<system-name>/general/mgmt/logs"
}
```

### get-config

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[String](../primitives.md#string)> as query parameter, which contains the names of the desired configuration properties.

```
GET /<system-specific-path>/general/mgmt/get-config?keys=management.policy&keys=max.page.size HTTP/1.1
Authorization: Bearer <identity-info>

```

The service operation **responds** with the status code `200` if called successfully. The response also contains an
[ConfigResponse](../data-models/config-response.md) JSON encoded body.

```
{
  "map": {
    "management.policy": "authorization",
    "max.page.size": "1000"
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.
```
{
  "errorMessage": "Invalid identity token",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```