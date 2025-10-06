# translationBridgeManagement IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of translationBridgeManagement, which enables to find providers whose
service operation is accessible by a specified consumer via a translation bridge using the currently available translators. The service also allows to build such bridges, to query or to abort existing translation bridges. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

Hereby the **Interface Design Description** (IDD) is provided to the [translationBridgeManagement – Service Description](../../assets/sd/5_1_0/translation-bridge-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### discovery

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [TranslationDiscoveryMgmtRequest](../data-models/translation-discovery-mgmt-request.md)
JSON encoded body.

TODO continue from here

```
POST /translation/bridge/discovery HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "candidates": [
    {
      "instanceId": "DoubleProvider|doubleService|1.0.0",
      "provider": {
        "name": "DoubleProvider"
      },
      "serviceDefinition": {
        "name": "doubleService"
      },
      "interfaces": [
        {
          "templateName": "generic_http",
          "policy": "USAGE_LIMITED_TOKEN_AUTH",
          "properties": {
            "accessAddresses": [
              "127.0.0.1"
            ],
            "accessPort": 23456,
            "operations": {
              "make-double": {
                "path": "/make-double",
                "method": "POST"
              }
            },
            "basePath": "/double",
            "dataModels": {
              "make-double": {
                "input": "testXml01",
                "output": "testXml01"
              }
            }
          }
        }
      ]
    }
  ],
  "operation": "make-double",
  "interfaceTemplateNames": [
    "generic_http"
  ],
  "inputDataModelId": "testJson01",
  "outputDataModelId": "testJson01"
}
```

> Note: The service operation can only work on candidates with interfaces that describe their data models. Data models must describe as a [DataModelMap](../data-models/data-model-map.md) inside the interface _properties_. See the example above.

The service operation **responds** with the status code `200` and with a [TranslationDiscoveryResponse](../data-models/translation-discovery-response.md) JSON encoded body.

```
{
  "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
  "candidates": [
    {
      "serviceInstanceId": "DoubleProvider|doubleService|1.0.0",
      "interfaceTemplateName": "generic_http"
    }
  ]
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission, `500` if an unexpected internal error happens
and `503` if an unexpected error happens while communicating other systems. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "operation is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST  /translation/bridge/discovery"
}
```

### negotiation

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [TranslationNegotiationRequest](../data-models/translation-negotiation-request.md)
JSON encoded body.

Operation _negotiation_ can be used two ways. 

- A. Using the results of a previously executed _discovery_ by specifing a valid _bridgeId_.
- B. Creating the _discovery_ results during the _negotiation_ by not specifing a valid _bridgeId_. This case requires additional information about the translation task in the request.

#### Example request for case A

```
POST /translation/bridge/negotiation HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
  "target": {
    "instanceId": "DoubleProvider|doubleService|1.0.0"
  }
}
```

#### Example request for case B

```
POST /translation/bridge/negotiation HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "target": {
    "instanceId": "DoubleProvider|doubleService|1.0.0",
    "provider": {
      "name": "DoubleProvider"
      
    },
    "serviceDefinition": {
      "name": "doubleService"
    },
    "interfaces": [
      {
        "templateName": "generic_http",
        "policy": "USAGE_LIMITED_TOKEN_AUTH",
        "properties": {
          "accessAddresses": [
            "127.0.0.1"
          ],
          "accessPort": 23456,
          "operations": {
            "make-double": {
              "path": "/make-double",
              "method": "POST"
            }
          },
          "basePath": "/double",
          "dataModels": {
            "make-double": {
              "input": "testXml01",
              "output": "testXml01"
            }
          }
        }
      }
    ]
  },
  "operation": "make-double",
  "interfaceTemplateName": "generic_http",
  "inputDataModelId": "testJson01",
  "outputDataModelId": "testJson01"
}
```

The service operation **responds** with the status code `201` if called successfully and with a [TranslationNegotiationResponse](../data-models/translation-negotiation-response.md) JSON encoded body.

```
{
  "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
  "bridgeInterface": {
    "templateName": "generic_http",
    "protocol": "http",
    "policy": "TRANSLATION_BRIDGE_TOKEN_AUTH",
    "properties": {
      "dataModels": {
        "make-double": {
          "input": "testJson01",
          "output": "testJson01"
        }
      },
      "operations": {
        "make-double": {
          "path": "/83a22bd8-d189-4c50-ba1a-ff5b32a2e3ca",
          "method": "POST"
        }
      },
      "basePath": "/interface/translator/dynamic",
      "accessAddresses": [
        "127.0.0.1"
      ],
      "accessPort": 12501
    }
  },
  "tokenUsageLimit": 5
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission, `500` if an unexpected internal error happens
and `503` if an unexpected error happens while communicating other systems. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Service instance id is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST  /translation/bridge/negotiation"
}
```

### abort

The service operation **request**  requires an [identity related header or certificate](../authentication_policy.md/#http) and a [BridgeIdentifier](../primitives.md#bridgeidentifier) as path parameter, which is a unique identifier of the translation bridge to be aborted.

```
DELETE /translation/bridge/abort/2240efa3-fde4-4f81-a625-04f1234acee7 HTTP/1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with the status code `200` if called successfully and an existing translation bridge was aborted and `204` if no translation bridge was found. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission, `500` if an unexpected internal error happens
and `503` if an unexpected error happens while communicating other systems. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "No permission to abort bridge: 2240efa3-fde4-4f81-a625-04f1234acee7",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN",
  "origin": "DELETE  /translation/bridge/abort/2240efa3-fde4-4f81-a625-04f1234acee7"
}
```