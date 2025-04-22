# general-management IDD
**GENERIC-MQTT & GENERIC-MQTTS**

## Overview

This page describes the GENERIC-MQTT and GENERIC-MQTTS service interface of general-management which allows (with operator role or proper permissions)
to get some information about the hosting system’s behavior, such as log entries and configuration settings. It is implemented using protocol, encoding as stated in the
following tables:

**GENERIC-MQTT**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**GENERIC-MQTTS**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 3.1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [general-management – Service Description](../../assets/sd/5_0_0/general-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### get-log

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [LogRequest](../data-models/log-request.md).


```
Topic: arrowhead/authentication/general/management/get-log

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": {
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
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a
[LogResponse](../data-models/log-response.md).

```
{
  "status": "200",
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
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
}
```
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": "400",
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Invalid time interval",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/authentication/general/management/get-log"
  }
}
```

### get-config

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[Name](../primitives.md#name)>, which contains the names of the desired configuration properties.

```
Topic: arrowhead/authentication/general/management/get-config

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": "<0|1|2>",
  "payload": [ "management.policy", "max.page.size" ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a
[ConfigResponse](../data-models/config-response.md).


```
{
  "status": "200",
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "map": {
      "management.policy": "authorization",
      "max.page.size": "1000"
    }
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": "401",
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Invalid identity token",
    "errorCode": 401,
    "exceptionType": "AUTH"
  }
}
```