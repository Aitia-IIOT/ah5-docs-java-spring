# translationReport IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of translationReport, which allows to send events about the life cycle of
translation bridges. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

Hereby the **Interface Design Description** (IDD) is provided to the [translationReport – Service Description](../../assets/sd/5_1_0/translation-report_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### report

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [TranslationReportRequest](../data-models/translation-report-request.md).

```
Topic: arrowhead/translation/event/report

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
     "timestamp": "2025-10-09T10:30:00Z",
     "state": "USED"
   }
}
```
The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.


```
{
  "status": 403,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
     "errorMessage": "Requester has no permission to report about the specified translation bridge",
     "errorCode": 403,
     "exceptionType": "FORBIDDEN",
     "origin": "arrowhead/translation/event/report"
   }
}
```