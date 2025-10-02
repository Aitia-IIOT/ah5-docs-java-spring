# dataModelTranslation IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of dataModelTranslation, which enables translation between different data models.

Hereby the **Interface Design Description** (IDD) is provided to the [dataModelTranslation – Service Description](../../assets/sd/5_1_0/dataModelTranslation_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### init-translation

The service operation **request** may require an [access token](../primitives.md#accesstoken) (depending on the applied [service security](../../help/service-security.md)) and a [DataModelTranslationInitRequest](../data-models/data-model-translation-init-request.md) JSON encoded body.

```
POST /<path>/<to>/<init> HTTP/1.1
Authorization: Bearer <access-token>

{
   "inputModelId":"abcJson",
   "outputModelId":"abcXml",
   "payload":"RG8geW91IGhhdmUgdG8gZGVhbCB3aXRoIEJhc2U2NCBmb3J...",
   "settings":{
      "foo":{
         "bar":12345
      }
   }
}
```

The service operation **responds** with the status code `200` if called successfully and the translation task has been initialized. The response also contains a
[DataModelTranslationTaskID](../primitives.md#datamodeltranslationtaskid.md) plain text body.

```
<any-unique-string>
```

The **error codes** are `400` if the request is malformed, `401` if credentials were requested but not presented, `403` if the access validation failed,
`500` if an unexpected error happens and `503` if the provider is overloaded with tasks. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":  "Translation payload is empty!",
  "errorCode":  400,
  "exceptionType":  "INVALID_PARAMETER",
  "origin":  "POST /<path>/<to>/<init>"
}
```

### get-translation-result

The service operation **request** may require an [access token](../primitives.md#accesstoken) (depending on the applied [service security](../../help/service-security.md)) and a [DataModelTranslationTaskID](../primitives.md#datamodeltranslationtaskid) as "_taskId_" query parameter.

```
GET /<path>/<to>/<get-result>?taskId=<unique-task-id-string> HTTP/1.1
Authorization: Bearer <access-token>
```

The service operation **responds** with the status code `200` if called successfully and the translation task exists. The response also contains a
[DataModelTranslationResultResponse](../data-models/data-translation-result-response.md) JSON encoded body.

```
{
   "status":"DONE",
   "result":"ZGVhbCB3aXRoIEJhc2U2NCBmb3JRG8geW91IGhhdmUgdG8g...",
   "mimeType":"text/xml"
}
```

The **error codes** are `400` if the request is malformed or "_taskId_" is missing, `401` if credentials were requested but not presented, `403` if the access validation failed,
`404` if the "_taskId_" in unkown and `500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":  "Task ID is missing!",
  "errorCode":  400,
  "exceptionType":  "INVALID_PARAMETER",
  "origin":  "GET /<path>/<to>/<get-result>"
}
```

### abort-translation

The service operation **request** may require an [access token](../primitives.md#accesstoken) (depending on the applied [service security](../../help/service-security.md)) and a [DataModelTranslationTaskID](../primitives.md#datamodeltranslationtaskid) as "_taskId_" query parameter.

```
DELETE /<path>/<to>/<abort>?taskId=<unique-task-id-string> HTTP/1.1
Authorization: Bearer <access-token>
```

The service operation **responds** with the status code `200` if called successfully and the translation task has been aborted.

The **error codes** are `400` if the request is malformed or "_taskId_" is missing, `401` if credentials were requested but not presented, `403` if the access validation failed,
`404` if the "_taskId_" in unkown and `500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":  "Task ID is missing!",
  "errorCode":  400,
  "exceptionType":  "INVALID_PARAMETER",
  "origin":  "DELETE /<path>/<to>/<abort>"
}
```