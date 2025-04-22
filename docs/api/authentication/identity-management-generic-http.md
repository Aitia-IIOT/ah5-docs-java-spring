# identity-management IDD
**GENERIC-HTTP & GENERIC-HTTPS**

## Overview

This page describes the GENERIC-HTTP and GENERIC-HTTPS service interface of identity-management which enables systems (with operator role or
proper permissions) to handle identities (create, update, remove, query) and active sessions (close, query) in bulk. It is implemented using protocol, encoding as stated in the
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

Hereby the **Interface Design Description** (IDD) is provided to the [identity-management – Service Description](../../assets/sd/5_0_0/identity-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### identity-mgmt-query

The service operation **request** requires an outsourced [identity related header](../authentication_policy.md/#outsourced-http) and an [IdentityQueryRequest](../data-models/identity-query-request.md) JSON encoded body.

```
POST /authentication/mgmt/identities/query HTTP/1.1
Authorization: Bearer <identity-info>

{
  "pagination": {
    "page": 0,
    "size": 10,
    "direction": "ASC",
    "sortField": "name"
  },
  "createdBy": "sysop",
  "creationFrom": "2025-03-07T06:00:00Z"
}
```

The service operation **responds** with the status code `200` if called successfully. The response also contains an
[IdentityListResponse](../data-models/identity-list-response.md) JSON encoded body.

```
{
  "identities": [
    {
      "systemName": "consumer1",
      "authenticationMethod": "PASSWORD",
      "sysop": false,
      "createdBy": "sysop",
      "createdAt": "2025-03-07T12:52:30Z",
      "updatedBy": "sysop",
      "updatedAt": "2025-03-07T12:52:30Z"
    },
    {
      "systemName": "provider1",
      "authenticationMethod": "PASSWORD",
      "sysop": false,
      "createdBy": "sysop",
      "createdAt": "2025-03-07T12:52:30Z",
      "updatedBy": "sysop",
      "updatedAt": "2025-03-07T12:52:30Z"
    }
  ],
  "count": 2
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "If size parameter is defined then page parameter cannot be undefined",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /authentication/mgmt/identities/query"
}
```

### identity-mgmt-create

The service operation **request** requires an outsourced [identity related header](../authentication_policy.md/#outsourced-http) and an [IdentityListCreateRequest](../data-models/identity-list-create-request.md) JSON encoded body.

```
POST /authentication/mgmt/identities HTTP/1.1
Authorization: Bearer <identity-info>

{
  "authenticationMethod": "PASSWORD",
  "identities": [
	{
	  "systemName": "consumer1",
	  "credentials": {
		"password": "abcdef"
	  },
	  "sysop": false
	},
	{
	  "systemName": "provider1",
	  "credentials": {
		"password": "123456"
	  },
	  "sysop": false
	}
  ]
}

```

The service operation **responds** with the status code `201` if called successfully. The response also contains an
[IdentityListResponse](../data-models/identity-list-response.md) JSON encoded body.

```
{
  "identities": [
    {
      "systemName": "consumer1",
      "authenticationMethod": "PASSWORD",
      "sysop": false,
      "createdBy": "sysop",
      "createdAt": "2025-03-07T12:52:30Z",
      "updatedBy": "sysop",
      "updatedAt": "2025-03-07T12:52:30Z"
    },
    {
      "systemName": "provider1",
      "authenticationMethod": "PASSWORD",
      "sysop": false,
      "createdBy": "sysop",
      "createdAt": "2025-03-07T12:52:30Z",
      "updatedBy": "sysop",
      "updatedAt": "2025-03-07T12:52:30Z"
    }
  ],
  "count": 2
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens.
If the Authentication System needs contacting an external server during the creation process, error code `503` can also be used if there was a problem with the external server. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Missing credentials",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /authentication/mgmt/identities"
}
```

### identity-mgmt-update

The service operation **request** requires an outsourced [identity related header](../authentication_policy.md/#outsourced-http) and an [IdentityListUpdateRequest](../data-models/identity-list-update-request.md) JSON encoded body.

```
PUT /authentication/mgmt/identities HTTP/1.1
Authorization: Bearer <identity-info>

{
  "identities": [
	{
	  "systemName": "consumer1",
	  "credentials": {
		"password": "123456"
	  },
	  "sysop": false
	},
	{
	  "systemName": "provider1",
	  "credentials": {
		"password": "123456"
	  },
	  "sysop": true
	}
  ]
}

```

The service operation **responds** with the status code `200` if called successfully. The response also contains an
[IdentityListResponse](../data-models/identity-list-response.md) JSON encoded body.

```
{
  "identities": [
    {
      "systemName": "consumer1",
      "authenticationMethod": "PASSWORD",
      "sysop": false,
      "createdBy": "sysop",
      "createdAt": "2025-03-07T12:52:30",
      "updatedBy": "sysop",
      "updatedAt": "2025-03-07T12:59:01"
    },
    {
      "systemName": "provider1",
      "authenticationMethod": "PASSWORD",
      "sysop": true,
      "createdBy": "sysop",
      "createdAt": "2025-03-07T12:52:30Z",
      "updatedBy": "sysop",
      "updatedAt": "2025-03-07T12:59:01Z"
    }
  ],
  "count": 2
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens.
If the Authentication System needs contacting an external server during the update process, error code `503` can also be used if there was a problem with the external server. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Missing credentials",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "PUT /authentication/mgmt/identities"
}
```

### identity-mgmt-remove

The service operation **request** requires an outsourced [identity related header](../authentication_policy.md/#outsourced-http) and a List<[Name](../primitives.md#name)> as query parameter, which contains the names of systems that needs to be removed.
```
DELETE /authentication/mgmt/identities?names=provider1&names=provider2 HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens.
If the Authentication System needs contacting an external server during the deletion process, error code `503` can also be used if there was a problem with the external server. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid identity token",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```

### identity-mgmt-session-query

The service operation **request** requires an outsourced [identity related header](../authentication_policy.md/#outsourced-http) and an [IdentitySessionQueryRequest](../data-models/identity-session-query-request.md) JSON encoded body.

```
POST /authentication/mgmt/sessions HTTP/1.1
Authorization: Bearer <identity-info>

{
  "pagination": {
    "page": 0,
    "size": 10,
    "direction": "ASC",
    "sortField": "name"
  },
  "loginFrom": "2025-03-07T10:00:00Z"
}
```

The service operation **responds** with the status code `200` if called successfully. The response also contains an
[IdentitySessionListResponse](../data-models/identity-session-list-response.md) JSON encoded body.

```
{
  "sessions": [
    {
      "systemName": "consumer1",
      "loginTime": "2025-03-07T11:54:01Z",
      "expirationTime": "2025-03-08T11:59:01Z"
    },
    {
      "systemName": "sysop",
      "loginTime": "2025-03-07T12:40:54Z",
      "expirationTime": "2025-03-08T12:45:54Z"
    }
  ],
  "count": 2
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "If size parameter is defined then page parameter cannot be undefined",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /authentication/mgmt/sessions"
}
```

### identity-mgmt-session-close

The service operation **request** requires an outsourced [identity related header](../authentication_policy.md/#outsourced-http) and a List<[Name](../primitives.md#name)> as query parameter, which contains the names of systems whose sessions needs to be closed.

```
DELETE /authentication/mgmt/sessions?names=consumer1 HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid identity token",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```