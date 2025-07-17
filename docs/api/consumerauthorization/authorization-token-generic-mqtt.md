# authorizationToken IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of authorizationToken, which enables the verification of service consumption permissions on the provider system side, and the application of session-based service consumption control between the consumer and provider systems. An example of this interaction is when a consumer system with proper permissions obtains an expiring token of the appropriate type, which is then attached to its service consumption attempt. The provider system verifies the received token and performs the service operation only if the token has not expired and is valid for the actual service operation. Additionally, provider systems can choose to have the tokens generated for their services encrypted. Tokens for Event notification are also handled by this service in an event publisher/subscriber scenario. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

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

Hereby the **Interface Design Description** (IDD) is provided to the [authorizationToken – Service Description](../../assets/sd/5_0_0/authorization-token_sd.pdf). For further details about how this service is meant to be used, please consult that document.

### generate

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationTokenGenerationRequest](../data-models/authorization-token-generation-request.md).

```
Topic: arrowhead/consumer-authorization/authorization-token/generate

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
      "tokenVariant": "USAGE_LIMITED_TOKEN_AUTH",
      "provider": "TemperatureProvider2",
      "targetType": "SERVICE_DEF",
      "target": "kelvinInfo",
      "scope": "query-temperature"
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if called successfully and token has been generated. The response template payload is an [AuthorizationTokenDescriptor](../data-models/authorization-token-descriptor.md).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "TemperatureConsumer",
   "payload" :{
      "tokenType": "USAGE_LIMITED_TOKEN",
      "targetType": "SERVICE_DEF",
      "token": "dsalefb521vdjkdsae633",
      "usageLimit": 10
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TemperatureConsumer",
   "payload": {
     "errorMessage": "Target is missing",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/consumer-authorization/authorization-token/generate"
   }
}
```

### verify

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AccessToken](../primitives.md#accesstoken).

```
Topic: arrowhead/consumer-authorization/authorization-token/verify

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": "dsalefb521vdjkdsae633"
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and the token has been checked. The response template payload is an [AuthorizationTokenVerifyResponse](../data-models/authorization-token-verify-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TemperatureProvider2",
   "payload": {
      "verified": true,
      "consumerCloud": "LOCAL",
      "consumer": "TemperatureConsumer",
      "targetType": "SERVICE_DEF",
      "target": "kelvinInfo",
      "scope": "query-temperature"
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TemperatureProvider2",
   "payload": {
     "errorMessage": "Self contained tokens can't be verified this way",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/consumer-authorization/authorization-token/verify"
   }
}
```

### get-public-key

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt).

```
Topic: arrowhead/consumer-authorization/authorization-token/get-public-key

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and the public key is retrieved. The response template payload is a [PublicKey](../primitives.md#publickey).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TemperatureProvider2",
   "payload": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA7D8z7doc95vY0uAx8JXwvrcl+Q7MykFoFIF1tn4fesvPIXo5eCGDS8FCONW0S5igQ+l00GdN/SlE0o85lI08TvepGEkTOtm1J+hsAHRD65OpPTjzWDVzP4+GzjZSUJl41iBDSW1YHgiFG8P2TqaTqNScrfLtKyekSzy/m24uh+zX5tjNoJ4GdSUeTNttHUuCH39MBxEo5E6KpzFGbC4105WHIH1MGWozOrZ3k7udvCLbCTvZ8PFtbDN4Ymjir0PE+6E2N4I+kagL1Py/DmNpKvLLI6m+YWJh2ErOAc56ThVvbCDeLOihacb26Y9Icrda1jOa30/xGsS3CmFLIpZjWwIDAQAB"
}
```

The **error codes** are `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `404` if public key is not available and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 404,
   "traceId": "<trace-id>",
   "receiver": "TemperatureProvider2",
   "payload": {
     "errorMessage": "Public key is not available",
     "errorCode": 404,
     "exceptionType": "DATA_NOT_FOUND",
     "origin": "arrowhead/consumer-authorization/authorization-token/get-public-key"
   }
}
```

### register-encryption-key

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [AuthorizationEncryptionKeyRegistrationRequest](../data-models/authorization-encryption-key-registration-request.md).

```
Topic: arrowhead/consumer-authorization/authorization-token/register-encryption-key

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
      "key": "zFGbC4105WHIH1MGWozOrZ3k7udv",
      "algorithm": "AES/CBC/PKCS5Padding"
   }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if called successfully and the record has been created. The response template payload might contain a Base64 [String](../primitives.md#string) cryptographic addition (depending on the specified [EncryptionAlgorithmName](../primitives.md#encryptionalgorithmname)).

```
{
   "status": 201,
   "traceId": "<trace-id>",
   "receiver": "TemperatureProvider2",
   "payload": "41iBDSW1YHgiFG8P2TqaTqNScrfLtKye"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TemperatureProvider2",
   "payload": {
     "errorMessage": "Unsupported algorithm",
     "errorCode": 400,
     "exceptionType": "INVALID_PARAMETER",
     "origin": "arrowhead/consumer-authorization/authorization-token/register-encryption-key"
   }
}
```

### unregister-encryption-key

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt).

```
Topic: arrowhead/consumer-authorization/authorization-token/unregister-encryption-key

{
   "traceId": "<trace-id>",
   "authentication": "<identity-info>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully and the associated record has been removed or `204` if there was no record associated with the requester.

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TemperatureProvider2"
}
```

The **error codes** are `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 401,
   "traceId": "<trace-id>",
   "receiver": "TemperatureProvider2",
   "payload": {
     "errorMessage": "No authentication info has been provided",
     "errorCode": 401,
     "exceptionType": "AUTH",
     "origin": "arrowhead/consumer-authorization/authorization-token/unregister-encryption-key"
   }
}
```