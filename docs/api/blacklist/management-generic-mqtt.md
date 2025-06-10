# management IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of management which enables which enables systems (with operator role or proper permissions) to handle (query, create, remove) blacklist entries.
This service interface is implemented using protocol, encoding as stated in the following tables:

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
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the management service.

## Interface Description

### query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [BlacklistQueryRequest](../data-models/blacklist-query-request.md) JSON encoded body.

```
Topic: arrowhead/blacklist/management/query

{
  "traceId": "<trace-id>",
  "authentication":"<identity-info>",
  "responseTopic":"<response-topic>",
  "qosRequirement":"<0|1|2>"
  "qosRequirement": 1,
  "payload": {
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
      "sysop"
    ],
    "revokers": [
    ],
    "reason": "temporary_ban",
    "alivesAt": "2025-06-05T23:59:59Z"
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [BlacklistEntryListResponse](../data-models/blacklist-entry-list-response.md) JSON encoded body.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "systemName": "AlertConsumer1",
        "createdBy": "Sysop",
        "createdAt": "2025-06-10T07:51:20Z",
        "updatedAt": "2025-06-10T07:51:20Z",
        "reason": "temporary_ban",
        "expiresAt": "2025-12-31T23:59:59Z",
        "active": true
      },
      {
        "systemName": "AlertConsumer2",
        "createdBy": "Sysop",
        "createdAt": "2025-06-10T07:51:20Z",
        "updatedAt": "2025-06-10T07:51:20Z",
        "reason": "temporary_ban",
        "expiresAt": "2025-12-31T23:59:59Z",
        "active": true
      }
    ],
    "count": 2
  }
}
```
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Mode is invalid. Possible values: ALL, ACTIVES, INACTIVES",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/blacklist/management/query"
  }
}
```

### create

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [BlacklistCreateListRequest](../data-models/blacklist-create-list-request.md) JSON encoded body.

```
Topic: arrowhead/blacklist/management/create

{
  "traceId": "<trace-id>",
  "authentication":"<identity-info>",
  "responseTopic":"<response-topic>",
  "qosRequirement":"<0|1|2>"
  "payload": {
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
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if called successfully. The response template payload is a [BlacklistEntryListResponse](../data-models/blacklist-entry-list-response.md) JSON encoded body.

```
{
  "status": 201,
  "traceId":"<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "systemName": "TemperatureProvider1",
        "createdBy": "Sysop",
        "createdAt": "2025-06-10T07:51:19.816605300Z",
        "updatedAt": "2025-06-10T07:51:19.816605300Z",
        "reason": "This provider is broken and sends too many false alarms. Should be fixed.",
        "active": true
      },
      {
        "systemName": "AlertConsumer1",
        "createdBy": "Sysop",
        "createdAt": "2025-06-10T07:51:20.296195800Z",
        "updatedAt": "2025-06-10T07:51:20.296195800Z",
        "reason": "temporary_ban",
        "expiresAt": "2025-12-31T23:59:59Z",
        "active": true
      },
      {
        "systemName": "AlertConsumer2",
        "createdBy": "Sysop",
        "createdAt": "2025-06-10T07:51:20.303375Z",
        "updatedAt": "2025-06-10T07:51:20.303375Z",
        "reason": "temporary_ban",
        "expiresAt": "2025-12-31T23:59:59Z",
        "active": true
      }
    ],
    "count": 3
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId":"<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": {
    "errorMessage": "You cannot blacklist a system without specifying the reason",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/blacklist/management/create"
  }
}
```

### remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and the payload is a List<[SystemName](../primitives.md#systemname)>, which contains the names of the systems to remove from the blacklist. This means that their active entries will be inactivated.

```
Topic: arrowhead/blacklist/management/remove

{
  "traceId": "<trace-id>",
  "authentication":"<identity-info>",
  "responseTopic":"<response-topic>",
  "qosRequirement":"<0|1|2>"
  "payload": ["AlertConsumer1", "AlertConsumer2"]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 403,
  "traceId": "<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": {
    "errorMessage": "TemperatureProvider1 system is blacklisted",
    "errorCode": 403,
    "exceptionType": "FORBIDDEN",
    "origin": "arrowhead/blacklist/management/remove"
  }
}
```

