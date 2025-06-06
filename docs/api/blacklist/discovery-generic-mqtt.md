# discovery IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of discovery which enables both application and Core/Support systems to query the blacklist entries in force that apply to them, or check if a system blacklisted.
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

Hereby the **Interface Design Description** (IDD) is provided to the discovery service.

## Interface Description

### lookup

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is empty. The name of the system whose active entries to lookup for will be identified during authentication.

```
Topic: arrowhead/blacklist/lookup

{
  "traceId": "<trace-id>",
  "authentication":"<identity-info>",
  "responseTopic":"<response-topic>",
  "qosRequirement":"<0|1|2>"
}
```
The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [BlacklistEntryListResponse](../data-models/blacklist-entry-list-response.md).

```
{
  "status":"<status-code>",
  "traceId":"<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": {
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
}
```
The **error codes** are `401` if the requester authentication was unsuccessful or `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status":"<status-code>",
  "traceId":"<trace-id>",
  "payload": {
    "errorMessage": "Invalid authentication info",
    "errorCode": 401,
    "exceptionType": "AUTH",
    "origin": "arrowhead/blacklist/lookup"
  }
}
```

### check

```
Topic: arrowhead/blacklist/check

{
  "traceId": "<trace-id>",
  "authentication":"<identity-info>",
  "responseTopic":"<response-topic>",
  "qosRequirement":"<0|1|2>"
  "payload": "AlertConsumer1"
}
```

```
{
  "status":"<status-code>",
  "traceId":"<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": false
}
```

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver":"<receiver-system-identifier>",
  "payload": {
    "errorMessage": "The specified system name does not match the naming convention: AlertCon$umer1",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/blacklist/check"
  }
}
```
