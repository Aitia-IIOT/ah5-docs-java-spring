# authorizationToken IDD
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of authorizationToken, which enables the verification of service consumption permissions on the provider system side, and the application of session-based service consumption control between the consumer and provider systems. An example of this interaction is when a consumer system with proper permissions obtains an expiring token of the appropriate type, which is then attached to its service consumption attempt. The provider system verifies the received token and performs the service operation only if the token has not expired and is valid for the actual service operation. Additionally, provider systems can choose to have the tokens generated for their services encrypted. Tokens for Event notification are also handled by this service in an event publisher/subscriber scenario. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

The interfaces are implemented using protocol, encoding as stated in the following tables:

## Interface Description

**generic_http**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTP | 1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**generic_https**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTPS | 1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [authorizationToken – Service Description](../../assets/sd/5_0_0/authorization-token_sd.pdf). For further details about how this service is meant to be used, please consult that document.

### generate

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationTokenGenerationRequest](../data-models/authorization-token-generation-request.md) JSON encoded body.


```
POST /consumerauthorization/authorization-token/generate HTTP/1.1
Authorization: Bearer <authorization-info>

{
   "tokenVariant": "USAGE_LIMITED_TOKEN_AUTH",
   "provider": "TemperatureProvider2",
   "targetType": "SERVICE_DEF",
   "target": "kelvinInfo",
   "scope": "query-temperature"
}
```

The service operation **responds** with `201` if called successfully and token has been generated. The response also contains an [AuthorizationTokenDescriptor](../data-models/authorization-token-descriptor.md) JSON encoded body.

```
{
   "tokenType": "USAGE_LIMITED_TOKEN",
   "targetType": "SERVICE_DEF",
   "token": "dsalefb521vdjkdsae633",
   "usageLimit": 10
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Target is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /consumerauthorization/authorization-token/generate"
}
```

### verify

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AccessToken](../primitives.md#accesstoken) path parameter.

```
GET /consumerauthorization/authorization-token/verify/dsalefb521vdjkdsae633 HTTP/1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with `200` if called successfully and token has been checked. The response also contains an [AuthorizationTokenVerifyResponse](../data-models/authorization-token-verify-response.md) JSON encoded body.

```
{
   "verified": true,
   "consumerCloud": "LOCAL",
   "consumer": "TemperatureConsumer",
   "targetType": "SERVICE_DEF",
   "target": "kelvinInfo",
   "scope": "query-temperature"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Self contained tokens can't be verified this way",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "GET /consumerauthorization/authorization-token/verify"
}
```

### get-public-key

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http).

```
GET /consumerauthorization/authorization-token/public-key HTTP/1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with `200` if called successfully and public key is retrieved. The response also contains a [PublicKey](../primitives.md#publickey) plain text body.

```
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA7D8z7doc95vY0uAx8JXwvrcl+Q7MykFoFIF1tn4fesvPIXo5eCGDS8FCONW0S5igQ+l00GdN/SlE0o85lI08TvepGEkTOtm1J+hsAHRD65OpPTjzWDVzP4+GzjZSUJl41iBDSW1YHgiFG8P2TqaTqNScrfLtKyekSzy/m24uh+zX5tjNoJ4GdSUeTNttHUuCH39MBxEo5E6KpzFGbC4105WHIH1MGWozOrZ3k7udvCLbCTvZ8PFtbDN4Ymjir0PE+6E2N4I+kagL1Py/DmNpKvLLI6m+YWJh2ErOAc56ThVvbCDeLOihacb26Y9Icrda1jOa30/xGsS3CmFLIpZjWwIDAQAB
```

The **error codes** are `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `404` if public key is not available and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Public key is not available",
  "errorCode": 404,
  "exceptionType": "DATA_NOT_FOUND",
  "origin": "GET /consumerauthorization/authorization-token/public-key"
}
```

### register-encryption-key

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationEncryptionKeyRegistrationRequest](../data-models/authorization-encryption-key-registration-request.md) JSON encoded body.

```
POST /consumerauthorization/authorization-token/encryption-key
Authorization: Bearer <authorization-info>

{
  "key": "zFGbC4105WHIH1MGWozOrZ3k7udv",
  "algorithm": "AES/CBC/PKCS5Padding"
}
```

The service operation **responds** with `201` if called successfully and the record has been created. The response might contain a Base64 [String](../primitives.md#string) cryptographic addition plain text body (depending on the specified [EncryptionAlgorithmName](../primitives.md#encryptionalgorithmname)).

```
41iBDSW1YHgiFG8P2TqaTqNScrfLtKye
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Unsupported algorithm",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /consumerauthorization/authorization-token/encryption-key"
}
```

### unregister-encryption-key

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http).

```
DELETE /consumerauthorization/authorization-token/encryption-key
Authorization: Bearer <authorization-info>
```

The service operation **responds** with `200` if called successfully and the associated record has been removed or `204` if there was no record associated with the requester.

The **error codes** are `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "No authentication info has been provided",
  "errorCode": 401,
  "exceptionType": "AUTH",
  "origin": "DELETE /consumerauthorization/authorization-token/encryption-key"
}
```