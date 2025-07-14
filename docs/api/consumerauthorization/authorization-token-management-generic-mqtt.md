# authorizationTokenManagement IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of authorizationTokenManagement, which allows systems (with operator role or proper permission) to manage the service access tokens in bulk and on behalf of the consumer and provider systems. Access tokens enable the verification of service consumption permissions on the provider system side, and the application of session-based service consumption control between the consumer and provider systems. An example of this interaction when a Core/Support system generates tokens for a consumer system for multiple service instances. Tokens for Event notification are also handled by this service in an event publisher/subscriber scenario. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

The interfaces are implemented using protocol, encoding as stated in the following tables:

## Interface Description

**generic_mqtt**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -


**generic_mqtts**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [authorizationTokenManagement – Service Description](../../assets/sd/5_0_0/authorization-token-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

### generate-tokens

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationTokenGenerationListMgmtRequest](../data-models/authorization-token-generation-list-mgmt-request.md). The params can contain an optional [KeyValuePair](../primitives.md/#keyvaluepair) with the key "_unbound_" and a [Boolean](../primitives.md#boolean) value. If _unbound_ is true, the consumers' service permission check will be skipped if the requester system name is present in the _unbounded.token.generation.whitelist_ system configuration.

```
Topic: arrowhead/consumer-authorization/authorization-token/management/generate-tokens

{
   "traceId":"<trace-id>",
   "authentication":"<identity-info>",
   "responseTopic":"<response-topic>",
   "qosRequirement":<0|1|2>,
   "params": {
    "unbound":true,
   },
   "payload":{
      "list":[
         {
            "tokenVariant":"TIME_LIMITED_TOKEN_AUTH",
            "targetType":"SERVICE_DEF",
            "consumerCloud":"LOCAL",
            "consumer":"TemperatureConsumer",
            "provider":"TemperatureProvider1",
            "target":"kelvinInfo",
            "scope":"query-temperature",
            "expiresAt":"2025-06-18T13:51:20Z"
         }
      ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if called successfully and tokens have been generated. The response template payload is an [AuthorizationTokenListMgmtResponse](../data-models/authorization-token-list-mgmt-response.md).

```
{
   "status":201,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager",
   "payload":{
      "entries":[
         {
            "tokenType":"TIME_LIMITED_TOKEN",
            "variant":"TIME_LIMITED_TOKEN_AUTH",
            "token":"dsalefb521vdjkdsae633",
            "tokenReference":"a4626c853f0c0c8989757bb0ecc7b992",
            "requester":"TemperatureManager",
            "consumerCloud":"LOCAL",
            "consumer":"TemperatureConsumer",
            "provider":"TemperatureProvider1",
            "targetType":"SERVICE_DEF",
            "target":"kelvinInfo",
            "scope":"query-temperature",
            "createdAt":"2025-06-18T13:40:20Z",
            "expiresAt":"2025-06-18T13:51:20Z"
         }
      ],
      "count":1
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":400,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager",
   "payload":{
     "errorMessage":"Token variant is missing",
     "errorCode":400,
     "exceptionType":"INVALID_PARAMETER",
     "origin":"arrowhead/consumer-authorization/authorization-token/management/generate-tokens"
   }
}
```

### query-tokens

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationTokenQueryRequest](../data-models/authorization-token-query-request.md).

```
Topic: arrowhead/consumer-authorization/authorization-token/management/query-tokens

{
   "traceId":"<trace-id>",
   "authentication":"<identity-info>",
   "responseTopic":"<response-topic>",
   "qosRequirement":<0|1|2>,
   "payload":{
      "pagination":{
         "page":0,
         "size":10
      },
      "requester":"TemperatureManager",
      "tokenType":"TIME_LIMITED_TOKEN",
      "consumerCloud":"LOCAL",
      "consumer":"TemperatureConsumer",
      "provider":"TemperatureProvider1",
      "targetType":"SERVICE_DEF",
      "target":"kelvinInfo"
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and tokens have been queried. The response template payload is an [AuthorizationTokenListMgmtResponse](../data-models/authorization-token-list-mgmt-response.md).

```
{
   "status":200,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager",
   "payload":{
      "entries":[
         {
            "tokenType":"TIME_LIMITED_TOKEN",
            "variant":"TIME_LIMITED_TOKEN_AUTH",
            "token":"dsalefb521vdjkdsae633",
            "tokenReference":"a4626c853f0c0c8989757bb0ecc7b992",
            "requester":"TemperatureManager",
            "consumerCloud":"LOCAL",
            "consumer":"TemperatureConsumer",
            "provider":"TemperatureProvider1",
            "targetType":"SERVICE_DEF",
            "target":"kelvinInfo",
            "scope":"query-temperature",
            "createdAt":"2025-06-18T13:40:20Z",
            "expiresAt":"2025-06-18T13:51:20Z"
         }
      ],
      "count":1
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":400,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager",
   "payload":{
     "errorMessage":"Invalid token type: SOMETHING",
     "errorCode":400,
     "exceptionType":"INVALID_PARAMETER",
     "origin":"arrowhead/consumer-authorization/authorization-token/management/query-tokens"
   }
}
```

### revoke-tokens

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[String](../primitives.md#string)> which contains the references of token records to be removed.

```
Topic: arrowhead/consumer-authorization/authorization-token/management/revoke-tokens

{
   "traceId":"<trace-id>",
   "authentication":"<identity-info>",
   "responseTopic":"<response-topic>",
   "qosRequirement":<0|1|2>,
   "payload": [
    "a4626c853f0c0c8989757bb0ecc7b992", "a573c531f0b0b8789757bb0ecc5b882"
   ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and tokens have been revoked.

```
{
   "status":200,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":401,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager",
   "payload":{
     "errorMessage":"No authentication info has been provided",
     "errorCode":401,
     "exceptionType":"AUTH",
     "origin":"arrowhead/consumer-authorization/authorization-token/management/revoke-tokens"
   }
}
```

### add-encryption-keys

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationEncryptionKeyListRequest](../data-models/authorization-encryption-key-list-request.md).

```
Topic: arrowhead/consumer-authorization/authorization-token/management/add-encryption-keys

{
   "traceId":"<trace-id>",
   "authentication":"<identity-info>",
   "responseTopic":"<response-topic>",
   "qosRequirement":<0|1|2>,
   "payload":{
      "list":[
         {
            "systemName":"TemperatureProvider2",
            "key":"abc1234",
            "algorithm":"AES/ECB/PKCS5Padding"
         }
      ]
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if called successfully and enryption keys have been saved. The response template payload is an [AuthorizationEncryptionKeyListResponse](../data-models/authorization-encryption-key-list-response.md).

```
{
   "status":201,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager",
   "payload":{
      "entries":[
         {
            "systemName":"TemperatureProvider2",
            "rawKey":"abc1234",
            "algorithm":"AES/ECB/PKCS5Padding",
            "keyAdditive":"",
            "createdAt":"2025-06-18T13:51:20Z"
         }
      ],
      "count":1
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":400,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager",
   "payload":{
     "errorMessage":"Unsupported algorithm",
     "errorCode":400,
     "exceptionType":"INVALID_PARAMETER",
     "origin":"arrowhead/consumer-authorization/authorization-token/management/add-encryption-keys"
   }
}
```

### remove-encryption-keys

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[SystemName](../primitives.md#systemname)> which contains the names of the systems to which the keys to be deleted belong.

```
Topic: arrowhead/consumer-authorization/authorization-token/management/remove-encryption-keys

{
   "traceId":"<trace-id>",
   "authentication":"<identity-info>",
   "responseTopic":"<response-topic>",
   "qosRequirement":<0|1|2>,
   "payload": [
    "TemperatureProvider1", "TemperatureProvider2"
   ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully.

```
{
   "status":200,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status":401,
   "traceId":"<trace-id>",
   "receiver":"TemperatureManager",
   "payload":{
     "errorMessage":"No authentication info has been provided",
     "errorCode":401,
     "exceptionType":"AUTH",
     "origin":"arrowhead/consumer-authorization/authorization-token/management/remove-encryption-keys"
   }
}
```