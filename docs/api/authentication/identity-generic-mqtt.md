# identity IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of identity which enables both application and Core/Support systems to
get and release a proof of identity token which also can be verified. Furthermore, it also allows a system to
change its own credentials. It is implemented using protocol, encoding as stated in the
following tables:

**generic_mqtt**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**generic_mqtts**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [identity – Service Description](../../assets/sd/5_0_0/identity_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### login

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the payload is an [IdentityRequest](../data-models/identity-request.md).

```
Topic: arrowhead/authentication/identity/identity-login

{
  "traceId": "<trace-id>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": {
    "systemName": "Consumer1",
    "credentials": {
      "password": "abcdef"
    }
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is an
[IdentityLoginResponse](../data-models/identity-login-response.md).


```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "token": "713bca0b-c550-4cb9-ae60-4852b9ee3669",
    "expirationTime": "2025-03-07T11:59:01.178225900Z"
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`500` if an unexpected error happens. If the Authentication System needs contacting an external server during the login process,
error code `503` can also be used if there was a problem with the external server.  In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 401,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Invalid name and/or credentials",
    "errorCode": 401,
    "exceptionType": "AUTH",
    "origin": "arrowhead/authentication/identity/identity-login"
  }
}
```

### logout

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the payload is an [IdentityRequest](../data-models/identity-request.md).

```
Topic: arrowhead/authentication/identity/identity-logout

{
  "traceId": "<trace-id>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": {
    "systemName": "Consumer1",
    "credentials": {
      "password": "abcdef"
    }
  }
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
`500` if an unexpected error happens. If the Authentication system needs contacting an external server during the logout process,
error code `503` can also be used if there was a problem with the external server.  In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.


```
{
  "status": 401,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Invalid name and/or credentials",
    "errorCode": 401,
    "exceptionType": "AUTH",
    "origin": "arrowhead/authentication/identity/identity-logout"
  }
}
```

### change

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the payload is an [IdentityChangeRequest](../data-models/identity-change-request.md).

```
Topic: arrowhead/authentication/identity/identity-change-credentials

{
  "traceId": "<trace-id>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": {
    "systemName": "Consumer1",
    "credentials": {
      "password": "abcdef"
    },
    "newCredentials": {
      "password": "123456"
    }
  }
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
`500` if an unexpected error happens. If the Authentication System needs contacting an external server during the credential change process,
error code `503` can also be used if there was a problem with the external server. In these cases the response template payload is an
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
    "origin": "arrowhead/authentication/identity/identity-change-credentials"
  }
}
```

### verify

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper outsourced [identity info](../../api/authentication_policy.md/#mqtt) and the payload is the token (as a string) that has to be verified.


```
Topic: arrowhead/authentication/identity/identity-verify

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": "713bca0b-c550-4cb9-ae60-4852b9ee3669"
}

```

The service operation **responds** with the status code `200` if called successfully. The response template payload is an
[IdentityVerifyResponse](../data-models/identity-verify-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "verified": true,
    "systemName": "Consumer1",
    "sysop": false,
    "loginTime": "2025-03-07T11:54:01Z",
    "expirationTime": "2025-03-07T12:54:01Z"
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Missing identity token",
    "errorCode": 401,
    "exceptionType": "AUTH"
  }
}
```