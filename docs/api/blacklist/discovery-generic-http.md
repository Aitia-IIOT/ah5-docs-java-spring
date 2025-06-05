# discovery IDD
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of discovery which enables both application and Core/Support systems to query the active blacklist entries that apply to them, or check if a system blacklisted.
It is implemented using protocol, encoding as stated in the
following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the discovery service.

## Interface Description

### lookup

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http).

```
GET /blacklist/lookup HTTP/1.1
Authorization: Bearer <identity-info>
```

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

```
{
  "errorMessage": "Invalid authorization header",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```

### check

```
GET /blacklist/check/AlertConsumer1 HTTP/1.1
Authorization: Bearer <identity-info>
```

```
false
```

```
{
  "errorMessage": "The specified system name does not match the naming convention: AlertCon$umer1",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "GET /blacklist/check/{systemName}"
}
```