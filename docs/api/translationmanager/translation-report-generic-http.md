# translationReport IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of translationReport, which allows to send events about the life cycle of
translation bridges. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

Hereby the **Interface Design Description** (IDD) is provided to the [translationReport – Service Description](../../assets/sd/5_1_0/translation-report_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### report

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [TranslationReportRequest](../data-models/translation-report-request.md)
JSON encoded body.


```
POST /translation/event/report HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
  "timestamp": "2025-10-09T10:30:00Z",
  "state": "USED"
}
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Requester has no permission to report about the specified translation bridge",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN",
  "origin": "DELETE  /translation/event/report"
}
```