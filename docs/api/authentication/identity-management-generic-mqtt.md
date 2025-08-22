# identityManagement IDD
**generic_mgtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of identity-management which enables systems (with operator role or
proper permissions) to handle identities (create, update, remove, query) and active sessions (close, query) in bulk.

Hereby the **Interface Design Description** (IDD) is provided to the [identityManagement – Service Description](../../assets/sd/5_0_0/identity-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### identity-mgmt-query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper outsourced [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an an [IdentityQueryRequest](../data-models/identity-query-request.md).

```
Topic: arrowhead/authentication/identity/management/identity-mgmt-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": {
    "pagination": {
      "page": 0,
      "size": 10,
      "direction": "ASC",
      "sortField": "name"
    },
    "createdBy": "Sysop",
    "creationFrom": "2025-03-07T06:00:00Z"
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is an
[IdentityListResponse](../data-models/identity-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "identities": [
      {
        "systemName": "Consumer1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "Sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "Sysop",
        "updatedAt": "2025-03-07T12:52:30Z"
      },
      {
        "systemName": "Provider1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "Sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "Sysop",
        "updatedAt": "2025-03-07T12:52:30Z"
      }
    ],
    "count": 2
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "If size parameter is defined then page parameter cannot be undefined",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/authentication/identity/management/identity-mgmt-query"
  }
}
```

### identity-mgmt-create

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper outsourced [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [IdentityListCreateRequest](../data-models/identity-list-create-request.md).

```
Topic: arrowhead/authentication/identity/management/identity-mgmt-create

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": {
    "authenticationMethod": "PASSWORD",
    "identities": [
	  {
	    "systemName": "Consumer1",
	    "credentials": {
		  "password": "abcdef"
	    },
	    "sysop": false
	  },
	  {
	    "systemName": "Provider1",
	    "credentials": {
		  "password": "123456"
	    },
	    "sysop": false
	  }
    ]
  }
}

```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if called successfully. The response template payload is an
[IdentityListResponse](../data-models/identity-list-response.md).


```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "identities": [
      {
        "systemName": "Consumer1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "Sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "Sysop",
        "updatedAt": "2025-03-07T12:52:30Z"
      },
      {
        "systemName": "Provider1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "Sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "Sysop",
        "updatedAt": "2025-03-07T12:52:30Z"
      }
    ],
    "count": 2
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens.
If the Authentication System needs contacting an external server during the creation process, error code `503` can also be used if there was a problem with the external server. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Missing credentials",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/authentication/identity/management/identity-mgmt-create"
  }
}
```

### identity-mgmt-update

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper outsourced [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [IdentityListUpdateRequest](../data-models/identity-list-update-request.md).

```
Topic: arrowhead/authentication/identity/management/identity-mgmt-update

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": {
    "identities": [
	  {
	    "systemName": "Consumer1",
	    "credentials": {
		  "password": "123456"
	    },
	    "sysop": false
	  },
	  {
	    "systemName": "Provider1",
	    "credentials": {
		  "password": "123456"
	    },
	    "sysop": true
	  }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is an
[IdentityListResponse](../data-models/identity-list-response.md).

```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "identities": [
      {
        "systemName": "Consumer1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "Sysop",
        "createdAt": "2025-03-07T12:52:30",
        "updatedBy": "Sysop",
        "updatedAt": "2025-03-07T12:59:01"
      },
      {
        "systemName": "Provider1",
        "authenticationMethod": "PASSWORD",
        "sysop": true,
        "createdBy": "Sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "Sysop",
        "updatedAt": "2025-03-07T12:59:01Z"
      }
    ],
    "count": 2
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens.
If the Authentication System needs contacting an external server during the update process, error code `503` can also be used if there was a problem with the external server. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Missing credentials",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/authentication/identity/management/identity-mgmt-update"
  }
}
```

### identity-mgmt-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper outsourced [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[SystemName](../primitives.md#systemname)>, which contains the names of systems that need to be removed.

```
Topic: arrowhead/authentication/identity/management/identity-mgmt-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": [ "Provider1", "Provider2" ]
}
```

The service operation **responds** with the status code `200` if called successfully. The response template payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens.
If the Authentication System needs contacting an external server during the deletion process, error code `503` can also be used if there was a problem with the external server. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 401,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Invalid identity token",
    "errorCode": 401,
    "exceptionType": "AUTH"
  }
}
```

### identity-mgmt-session-query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper outsourced [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [IdentitySessionQueryRequest](../data-models/identity-session-query-request.md).

```
Topic: arrowhead/authentication/identity/management/identity-mgmt-session-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": {
    "pagination": {
      "page": 0,
      "size": 10,
      "direction": "ASC",
      "sortField": "name"
    },
    "loginFrom": "2025-03-07T10:00:00Z"
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is an
[IdentitySessionListResponse](../data-models/identity-session-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "sessions": [
      {
        "systemName": "Consumer1",
        "loginTime": "2025-03-07T11:54:01Z",
        "expirationTime": "2025-03-08T11:59:01Z"
      },
      {
        "systemName": "Sysop",
        "loginTime": "2025-03-07T12:40:54Z",
        "expirationTime": "2025-03-08T12:45:54Z"
      }
    ],
    "count": 2
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "If size parameter is defined then page parameter cannot be undefined",
	"errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/authentication/identity/management/identity-mgmt-session-query"
  }
}
```

### identity-mgmt-session-close

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper outsourced [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[SystemName](../primitives.md#systemname)>, which contains the names of systems whose sessions need to be closed.

```
Topic: arrowhead/authentication/identity/management/identity-mgmt-session-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": [ "Consumer1" ]
}
```

The service operation **responds** with the status code `200` if called successfully. The response template payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens.  In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 401,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Invalid identity token",
    "errorCode": 401,
    "exceptionType": "AUTH"
  }
}
```