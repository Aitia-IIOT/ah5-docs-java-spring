# interfaceBridgeManagement IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of interfaceBridgeManagement, which enables translation between different interfaces.

Hereby the **Interface Design Description** (IDD) is provided to the [interfaceBridgeManagement – Service Description](../../assets/sd/5_1_0/interfaceBridgeManagement_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### check-targets

The service operation **request** may require an [access token](../primitives.md#accesstoken) (depending on the applied [service security](../../help/service-security.md)) and a [TranslationCheckTargetsRequest](../data-models/translation-check-targets-request.md) JSON encoded body.

```
POST /<path>/<to>/<check> HTTP/1.1
Authorization: Bearer <access-token>

{
   "targetOperation": "query-temperature",
   "targets": [
      {
         "instanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
         "interfaces": [
            {
               "templateName": "generic_mqtt",
               "protocol": "mqtt",
               "policy": "TIME_LIMITED_TOKEN_AUTH",
               "properties": {
                  "accessAddresses": [
                     "192.168.56.116",
                     "tp2.greenhouse.com"
                  ],
                  "accessPort": 8080,
                  "basePath": "/kelvin/",
                  "operations": [
                    "query-temperature"
                  ],
                  "dataModels": {
                    "query-temperature": {
                        "input": "abcXml",
                        "output" "xyzXml"
                    }
                  }
               }
            }
         ]
      }
   ]
}
```

The service operation **responds** with the status code `200` if called successfully and the tragtes have been filtered. The response also contains a
[TranslationCheckTargetsResponse](../data-models/translation-check-targets-response.md) JSON encoded body.

```
{
    "targets": [
      {
         "instanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
         "interfaces": [
            {
               "templateName": "generic_mqtt",
               "protocol": "mqtt",
               "policy": "TIME_LIMITED_TOKEN_AUTH",
               "properties": {
                  "accessAddresses": [
                     "192.168.56.116",
                     "tp2.greenhouse.com"
                  ],
                  "accessPort": 8080,
                  "basePath": "/kelvin/",
                  "operations": [
                    "query-temperature"
                  ],
                  "dataModels": {
                    "query-temperature": {
                        "input": "abcXml",
                        "output" "xyzXml"
                    }
                  }
               }
            }
         ]
      }
   ]
}
```

The **error codes** are `400` if the request is malformed, `401` if credentials were requested but not presented, `403` if the access validation failed,
`500` if an unexpected error happens and `503` if the provider is overloaded with tasks. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":  "Target operation is missing",
  "errorCode":  400,
  "exceptionType":  "INVALID_PARAMETER",
  "origin":  "POST /<path>/<to>/<check>"
}
```

### initialize-bridge

The service operation **request** may require an [access token](../primitives.md#accesstoken) (depending on the applied [service security](../../help/service-security.md)) and a [TranslationBridgeInitializationRequest](../data-models/translation-bridge-initialization-request.md) JSON encoded body.

```
POST /<path>/<to>/<init> HTTP/1.1
Authorization: Bearer <access-token>

{
   "bridgeId": "4d1dcec8-e375-448b-bb01-95e9e9d5ee35",
   "operation": "query-temperature",
   "inputInterface": "generic_http",
   "targetInterface": "generic_mqtt",
   "targetInterfaceProperties": {
      "accessAddresses": [
         "92.168.56.116",
         "tp2.greenhouse.com"
      ],
      "accessPort": 8080,
      "basePath": "/kelvin/",
      "operations": [
         "query-temperature"
      ],
      "dataModels": {
          "query-temperature": {
             input": "abcXml",
             "output" "xyzXml"
           }
       }
   },
   "inputDataModelRequirement": "abcJson",
   "inputDataModelTranslator": {
      "fromModelId": "abcJson",
      "toModelId": "abcXml",
      "interfaceProperties": {
         "accessAddresses": [
            "92.168.56.103"
         ],
         "accessPort": 8080,
         "basePath": "/model-translation/",
         "operations": {
            "init-translation": {
               "path": "/init",
               "method": "POST"
            },
            "get-translation-result": {
               "path": "/result",
               "method": "GET"
            },
            "abort-translation": {
               "path": "/abort",
               "method": "DELETE"
            }
         }
      },
      "configurationSettings": {
         "foo":"bar"
      }
   },
   "resultDataModelRequirement":" xyzJson",
   "outputDataModelTranslator": {
      "fromModelId": "xyzXml",
      "toModelId": "xyzJson",
      "interfaceProperties": {
         "accessAddresses": [
            "92.168.56.103"
         ],
         "accessPort": 8080,
         "basePath": "/model-translation/",
         "operations": {
            "init-translation": {
               "path": "/init",
               "method": "POST"
            },
            "get-translation-result": {
               "path": "/result",
               "method": "GET"
            },
            "abort-translation": {
               "path": "/abort",
               "method": "DELETE"
            }
         }
      },
      "configurationSettings": {
         "foo": "bar"
      }
   },
   "authorizationToken": "lkjvdslkangSJKkdsnfcdov...",
   "interfaceTranslatorSettings": {
      "bar": "foo"
   }
}
```

The service operation **responds** with the status code `200` if called successfully and the translation bridge has been initialized. The response also contains a
[ServiceInterfaceDescriptor](../data-models/service-interface-descriptor.md) JSON encoded body.

```
{
   "templateName":"generic_http",
   "protocol":"http",
   "policy":"TRANSLATION_BRIDGE_TOKEN_AUTH",
   "properties":{
      "accessAddresses":[
         "192.168.56.101"
      ],
      "accessPort":12501,
      "operations":{
         "query-temperature":{
            "path":"/1b376055-7b1b-4c40-8de8-96851957b86c",
            "method":"GET"
         },
         "dataModels": {
            "query-temperature": {
               "input": "abcJson",
               "output" "xyzJson"
            }
         }
      },
      "basePath":"/dynamic"
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if credentials were requested but not presented, `403` if the access validation failed,
`500` if an unexpected error happens and `503` if the provider is overloaded with tasks. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":  "Input interface name is missing",
  "errorCode":  400,
  "exceptionType":  "INVALID_PARAMETER",
  "origin":  "POST /<path>/<to>/<init>"
}
```

### abort-translation

The service operation **request** may require an [access token](../primitives.md#accesstoken) (depending on the applied [service security](../../help/service-security.md)) and a [TranslationBridgeID](../primitives.md#translationbridgeid) as path parameter.

```
DELETE /<path>/<to>/<abort>/4d1dcec8-e375-448b-bb01-95e9e9d5ee35 HTTP/1.1
Authorization: Bearer <access-token>
```

The service operation **responds** with the status code `200` if called successfully and the translation task has been aborted or with `204` if the referenced translation bridge not exists.

The **error codes** are `400` if the request is malformed or bridge id is missing, `401` if credentials were requested but not presented, `403` if the access validation failed, `500` if an unexpected error happens and `503` if an external error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":  "Bridge identifier is invalid",
  "errorCode":  400,
  "exceptionType":  "INVALID_PARAMETER",
  "origin":  "DELETE /<path>/<to>/<abort>"
}
```