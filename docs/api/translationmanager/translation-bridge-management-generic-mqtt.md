# translationBridgeManagement IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of translationBridgeManagement, which enables to find providers whose
service operation is accessible by a specified consumer via a translation bridge using the currently available translators. The service also allows to build such bridges, to query or to abort existing translation bridges. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

Hereby the **Interface Design Description** (IDD) is provided to the [translationBridgeManagement – Service Description](../../assets/sd/5_1_0/translation-bridge-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### discovery

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [TranslationDiscoveryMgmtRequest](../data-models/translation-discovery-mgmt-request.md).

```
Topic: arrowhead/translation/bridge/management/discovery 

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
     "consumer": "TestConsumer",
     "operation": "make-double",
     "interfaceTemplateNames": [
       "generic_mqtt"
     ],
     "inputDataModelId": "testJson01",
     "outputDataModelId": "testJson01",
	 "flags": {
       "CONSUMER_BLACKLIST_CHECK": false,
	   "CANDIDATES_BLACKLIST_CHECK": false,
	   "CANDIDATES_AUTH_CHECK": true,
	   "TRANSLATORS_BLACKLIST_CHECK": false,
	   "TRANSLATORS_AUTH_CHECK": true
     }
  }
}
```

> Note: The service operation can only work on candidates with interfaces that describe their data models. Data models must be described as a [DataModelMap](../data-models/data-model-map.md) inside the interface _properties_. See the example above.

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
and `503` if an unexpected error happens while communicating with other systems.  In these cases the response template payload is an
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
     "origin": "arrowhead/translation/bridge/management/discovery"
   }
}
```

### negotiation

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [TranslationNegotiationMgmtRequest](../data-models/translation-negotiation-mgmt-request.md) JSON encoded body.

```
Topic: arrowhead/translation/bridge/management/negotiation

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
     "targetInstanceId": "DoubleProvider|doubleService|1.0.0"
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
and `503` if an unexpected error happens while communicating with other systems. In these cases the response template payload is an
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
     "origin": "arrowhead/translation/bridge/management/negotiation"
   }
}
```

### abort

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[TranslationBridgeID](../primitives.md#translationbridgeid)>, which contains the identitifers of the translation bridgies that need to aborted.

```
Topic:  arrowhead/translation/bridge/management/abort

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": [
     "2240efa3-fde4-4f81-a625-04f1234acee7",
	 "f098f234-e071-4e60-9827-b67656b2cc82"
   ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is  a  [TranslationAbortMgmtResponse](../data-models/translation-abort-mgmt-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "results": {
       "2240efa3-fde4-4f81-a625-04f1234acee7": true,
	   "f098f234-e071-4e60-9827-b67656b2cc82": false
     }
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission, `500` if an unexpected internal error happens
and `503` if an unexpected error happens while communicating with other systems. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "errorMessage": "Bridge identifier is invalid: 2240efa3-fde4-4f81-a625-04f1234acee",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/translation/bridge/management/abort"
   }
}
```

### query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [TranslationQueryRequest](../data-models/translation-query-request.md).

```
Topic: arrowhead/translation/bridge/management/query

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": "<0|1|2>",
   "payload": {
     "pagination": {
       "page": 0,
       "size": 2,
       "direction": "ASC",
       "sortField": "createdAt"
     },
     "bridgeIds": [ "2240efa3-fde4-4f81-a625-04f1234acee7" ],
     "creators": [ "TestConsumer" ],
     "statuses": [ "INITIALIZED", "USED" ],
     "consumers": [ "TestConsumer" ],
     "providers": [ "DoubleProvider" ],
     "serviceDefinitions": [ "doubleService" ],
     "interfaceTranslators": [ "InterfaceTranslatorToGenericHTTP" ],
     "dataModelTranslators": [ "DummyJsonXmlTranslator" ],
     "creationFrom": "2025-10-07T10:00:00Z",
     "creationTo": "2025-10-07T19:00:00Z",
     "alivesFrom": "2025-10-07T11:00:00Z",
     "alivesTo": "2025-10-07T13:00:00Z"
     "minUsage": 1,
     "maxUsage": 100
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [TranslationQueryListResponse](../data-models/translation-query-list-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "entries": [
       {
	     "bridgeId": "2240efa3-fde4-4f81-a625-04f1234acee7",
	     "status": "USED",
	     "usageReportCount": 13,
	     "alivesAt": "2025-10-07T12:46:12Z",
	     "consumer": "TestConsumer",
	     "provider": "DoubleProvider",
	     "serviceDefinition": "doubleService",
	     "operation": "make-double",
	     "interfaceTranslator": "InterfaceTranslatorToGenericHTTP",
	     "interfaceTranslatorData": {
	       "fromInterfaceTemplate": "generic_mqtt",
		   "toInterfaceTemplate": "generic_http",
		   "token": "BjS7VQTRXfsBQngr8eM02a1ZB8Zt3jxr5XefhKqfadZLTOHDH4amxcskXLRGRsoW5upmiX1d+MPZ4S6R2wltFUUROH6SXPoue+OReSfoUeGgcFDxVe9RFXEmxCpRQPoMhwk2FyGlMmNMIAUHCU++MW9iuGzlARNrfOl3UUF14H+ch0GopXQYDB6KxzRF8iHTXQ9OEK//M9ZRbSW0QXXoyRZMa73VB1PHL+yaTFrKWBw=",
		   "interfaceProperties": {
             "accessAddresses": [
                "127.0.0.1"
             ],
             "accessPort": 12501,
             "operations": {
               "initialize-bridge": {
                 "path": "/initialize-bridge",
                 "method": "POST"
               },
               "abort-bridge": {
                 "path": "/abort-bridge",
                 "method": "DELETE"
               },
               "check-targets": {
                 "path": "/check-targets",
                 "method": "POST"
               }
             },
             "basePath": "/interface/translator/bridge/mgmt"
           }
	     },
	     "inputDataModelTranslator": "DummyJsonXmlTranslator",
	     "inputDataModelTranslatorData": {
            "fromModelId": "testJson01",
            "toModelId": "testXml01",
            "interfaceProperties": {
              "accessAddresses": [
                 "127.0.0.1"
              ],
             "accessPort": 22222,
             "operations": {
               "abort-translation": {
                 "path": "/abort",
                 "method": "DELETE"
               },
               "init-translation": {
                  "path": "/init",
                  "method": "POST"
               },
               "get-translation-result": {
                 "path": "/get",
                 "method": "GET"
               }
             },
             "basePath": "/data-model/translation"
           }
         },
	     "outputDataModelTranslator": "DummyJsonXmlTranslator",
	     "outputDataModelTranslatorData": {
           "fromModelId": "testXml01",
           "toModelId": "testJson01",
           "interfaceProperties": {
             "accessAddresses": [
                 "127.0.0.1"
             ],
             "accessPort": 22222,
             "operations": {
               "abort-translation": {
                 "path": "/abort",
                 "method": "DELETE"
               },
               "init-translation": {
                 "path": "/init",
                 "method": "POST"
               },
               "get-translation-result": {
                 "path": "/get",
                 "method": "GET"
               }
             },
             "basePath": "/data-model/translation"
           }
         },
	     "createdBy": "TestConsumer",
	     "createdAt": "2025-10-07T10:36:47Z",
	     "updatedAt": "2025-10-07T12:46:12Z"
 	   }
     ],
     "count": 1
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an
[ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "<receiver-system-identifier>",
   "payload": {
     "errorMessage": "Bridge id is invalid: 2240efa3-fde4-4f81-a625-04f1234acee",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/translation/bridge/management/discovery"
   }
}
```