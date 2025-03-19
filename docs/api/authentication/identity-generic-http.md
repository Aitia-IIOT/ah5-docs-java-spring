# identity IDD
**GENERIC-HTTP & GENERIC-HTTPS**

## Overview

This page describes the GENERIC-HTTP and GENERIC-HTTPS service interface of identity which enables both application and core/support systems to
get and release a proof of identity token which also can be verified. Furthermore, it also allows a system to
change its own credentials. It is implemented using protocol, encoding as stated in the
following tables:

**GENERIC-HTTP**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTP | 1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**GENERIC-HTTPS**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTPS | 1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [identity – Service Description](../../assets/sd/5_0_0/identity_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### login

The service operation **request** requires an [IdentityRequest](../data-models/identity-request.md) JSON encoded body.

```
POST /authentication/identity/login HTTP/1.1

{
  "systemName": "consumer1",
  "credentials": {
    "password": "abcdef"
  }
}

```

The service operation **responds** with the status code `200` if called successfully. The response also contains an
[IdentityLoginResponse](../data-models/identity-response.md) JSON encoded body.

```
{
  "token": "713bca0b-c550-4cb9-ae60-4852b9ee3669",
  "expirationTime": "2025-03-07T11:59:01.178225900Z"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`500` if an unexpected error happens. If the Authentication system needs contacting an external server during the login process,
error code `503` can also be used if there was a problem with the external server.  The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid name and/or credentials",
  "errorCode": 401,
  "exceptionType": "AUTH",
  "origin": "POST /authentication/identity/login"
}
```

### logout

The service operation **request** requires an [IdentityRequest](../data-models/identity-request.md) JSON encoded body.

```
POST /authentication/identity/logout HTTP/1.1

{
  "systemName": "consumer1",
  "credentials": {
    "password": "abcdef"
  }
}

```

The service operation **responds** with the status code `200` if called successfully. The response does not contain any
response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`500` if an unexpected error happens. If the Authentication system needs contacting an external server during the logout process,
error code `503` can also be used if there was a problem with the external server.  The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid name and/or credentials",
  "errorCode": 401,
  "exceptionType": "AUTH",
  "origin": "POST /authentication/identity/logout"
}
```

### change

The service operation **request** requires an [IdentityChangeRequest](../data-models/identity-change-request.md) JSON encoded body.

```
POST /authentication/identity/change HTTP/1.1

{
  "systemName": "consumer1",
  "credentials": {
    "password": "abcdef"
  },
  "newCredentials": {
    "password": "123456"
  }
}
```

The service operation **responds** with the status code `200` if called successfully. The response does not contain any
response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`500` if an unexpected error happens. If the Authentication system needs contacting an external server during the credential change process,
error code `503` can also be used if there was a problem with the external server.  The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Missing credentials",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /authentication/identity/change"
}
```

### verify

The service operation **request** requires an outsourced [identity related header](../authentication_policy.md/#outsourced-http) and the `token` that has to be verified as a path parameter.


```
GET /authentication/identity/verify/713bca0b-c550-4cb9-ae60-4852b9ee3669 HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully. The response also contains an
[IdentityVerifyResponse](../data-models/identity-verify-response.md) JSON encoded body.

```
{
  "verified": true,
  "systemName": "consumer1",
  "sysop": false,
  "loginTime": "2025-03-07T11:54:01Z",
  "expirationTime": "2025-03-07T12:54:01Z"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "No authorization header has been provided",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```