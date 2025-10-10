# translationBridge IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of translationBridge, which enables to find providers whose service
operation is accessible by the requester via a translation bridge using the currently available translators. The service also allows to build or abort such bridges. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

Hereby the **Interface Design Description** (IDD) is provided to the [translationBridge – Service Description](../../assets/sd/5_1_0/translation-bridge_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### discovery

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [TranslationDiscoveryRequest](../data-models/translation-discovery-request.md).

```
Topic: arrowhead/translation/bridge/discovery

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
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
       "generic_mqtt"
     ],
     "inputDataModelId": "testJson01",
     "outputDataModelId": "testJson01"
  }
}
```

> Note: The service operation can only work on candidates with interfaces that describe their data models. Data models must describe as a [DataModelMap](../data-models/data-model-map.md) inside the interface _properties_. See the example above.

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [TranslationDiscoveryResponse](../data-models/translation-discovery-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
     "candidates": [
       {
         "serviceInstanceId": "DoubleProvider|doubleService|1.0.0",
         "interfaceTemplateName": "generic_http"
       }
     ]
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission, `500` if an unexpected internal error happens
and `503` if an unexpected error happens while communicating other systems. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.


```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "errorMessage": "operation is missing",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/translation/bridge/discovery"
   }
}
```

### negotiation

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [TranslationNegotiationRequest](../data-models/translation-negotiation-request.md).

Operation _negotiation_ can be used two ways. 

- A. Using the results of a previously executed _discovery_ by specifing a valid _bridgeId_.
- B. Creating the _discovery_ results during the _negotiation_ by not specifing a valid _bridgeId_. This case requires additional information about the translation task in the request.

#### Example request for case A

```
Topic: arrowhead/translation/bridge/negotiation

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
     "target": {
       "instanceId": "DoubleProvider|doubleService|1.0.0"
     }
   }
}
```

#### Example request for case B

```
Topic: arrowhead/translation/bridge/negotiation

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
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
     "interfaceTemplateName": "generic_mqtt",
     "inputDataModelId": "testJson01",
     "outputDataModelId": "testJson01"
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if called successfully. The response template payload is  a [TranslationNegotiationResponse](../data-models/translation-negotiation-response.md).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
     "bridgeInterface": {
       "templateName": "generic_mqtt",
       "protocol": "tcp",
       "policy": "TRANSLATION_BRIDGE_TOKEN_AUTH",
       "properties": {
         "dataModels": {
           "make-double": {
             "input": "testJson01",
             "output": "testJson01"
           }
         },
         "operations": [ "make-double" ],
         "baseTopic": "/interface/translator/dynamic/83a22bd8-d189-4c50-ba1a-ff5b32a2e3ca/",
         "accessAddresses": [
           "127.0.0.1"
         ],
         "accessPort": 1883
       }
     },
     "tokenUsageLimit": 5
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission, `500` if an unexpected internal error happens
and `503` if an unexpected error happens while communicating other systems.In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "errorMessage": "Service instance id is missing",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/translation/bridge/negotiation"
   }
}
```

### abort

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a a [TranslationBridgeID](../primitives.md#translationbridgeid), which is a unique identifier of the translation bridge to be aborted.


```
Topic: arrowhead/translation/bridge/abort

{
  "traceId": "<trace-id>",
  "authentication": "<authentication-data>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": "2240efa3-fde4-4f81-a625-04f1234acee7"
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and an existing translation bridge was aborted and `204` if no translation bridge was found.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission, `500` if an unexpected internal error happens
and `503` if an unexpected error happens while communicating other systems. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.


```
{
  "status": 403,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
     "errorMessage": "No permission to abort bridge: 2240efa3-fde4-4f81-a625-04f1234acee7",
     "errorCode": 403,
     "exceptionType": "FORBIDDEN",
     "origin": "arrowhead/translation/bridge/abort/"
   }
}
```