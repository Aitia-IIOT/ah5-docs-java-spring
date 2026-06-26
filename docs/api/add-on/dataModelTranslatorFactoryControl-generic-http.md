# dataModelTranslatorFactoryControl IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of dataModelTranslatorFactoryControl, which enables on-demand creation of DataModelTranslator providers.

Hereby the **Interface Design Description** (IDD) is provided to the [dataModelTranslatorFactoryControl – Service Description](../../assets/sd/5_2_1/dataModelTranslatorFactoryControl_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### check

The service operation **request** requires a [DataModelTranslationFactoryRequest](../data-models/data-model-translation-factory-request.md) JSON encoded body.

```
POST /<path>/<to>/<check> HTTP/1.1

{
   "fromModelId":"abcJson",
   "toModelId":"abcXml"
}
```

The service operation **responds** with the status code `200` if called successfully. The response also contains a [Boolean](../primitives.md#boolean) JSON encoded body.

```
"true"
```

The **error codes** are `400` if the request is malformed, `401` if credentials were requested but not presented, `403` if the access validation failed and
`500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":  "fromModelId is empty",
  "errorCode":  400,
  "exceptionType":  "INVALID_PARAMETER",
  "origin":  "POST /<path>/<to>/<check>"
}
```

### initialize

The service operation **request** requires a [DataModelTranslationFactoryRequest](../data-models/data-model-translation-factory-request.md) JSON encoded body.

```
POST /<path>/<to>/<initialize> HTTP/1.1

{
   "fromModelId":"abcJson",
   "toModelId":"abcXml"
}
```

The service operation **responds** with the status code `201` if called successfully and the required data model translator has been created. The response also contains a [DataModelTranslatorResult](../data-models/data-model-translator-result.md) JSON encoded body.

```
{
   "dataModelTranslatorName":"fc06eee2-3e8e-4d0b-81a2-00fcd845f67a",
   "interfaceProperties":{
      "accessAddresses":[
         "192.168.0.12"
      ],
      "accessPort":22222,
      "operations":{
         "abort-translation":{
            "path":"/abort",
            "method":"DELETE"
         },
         "init-translation":{
            "path":"/init",
            "method":"POST"
         },
         "get-translation-result":{
            "path":"/get",
            "method":"GET"
         }
      },
      "basePath":"/data-model/translation/fc06eee2-3e8e-4d0b-81a2-00fcd845f67a"
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if credentials were requested but not presented, `403` if the access validation failed and `500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":  "fromModelId is empty",
  "errorCode":  400,
  "exceptionType":  "INVALID_PARAMETER",
  "origin":  "POST /<path>/<to>/<initialize>"
}
```