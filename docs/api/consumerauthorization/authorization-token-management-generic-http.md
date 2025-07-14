# authorizationTokenManagement IDD
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of authorizationTokenManagement, which allows systems (with operator role or proper permission) to manage the service access tokens in bulk and on behalf of the consumer and provider systems. Access tokens enable the verification of service consumption permissions on the provider system side, and the application of session-based service consumption control between the consumer and provider systems. An example of this interaction when a Core/Support system generates tokens for a consumer system for multiple service instances. Tokens for Event notification are also handled by this service in an event publisher/subscriber scenario. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

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

Hereby the **Interface Design Description** (IDD) is provided to the [authorizationTokenManagement – Service Description](../../assets/sd/5_0_0/authorization-token-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

### generate-tokens

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationTokenGenerationListMgmtRequest](../data-models/authorization-token-generation-list-mgmt-request.md) JSON encoded body. The URI can contain an optional query parameter with the key "_unbound_" and a [Boolean](../primitives.md#boolean) value. If `true` the consumers' service permission check will be skipped if the requester system name is present in the _unbounded.token.generation.whitelist_ system cunfiguration. 

```
POST /consumerauthorization/authorization/mgmt/token/generate?unbound=true HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "list": [
    {
      "tokenVariant": "TIME_LIMITED_TOKEN_AUTH",
      "targetType": "SERVICE_DEF",
      "consumerCloud": "LOCAL",
      "consumer": "TemperatureConsumer",
      "provider": "TemperatureProvider1",
      "target": "kelvinInfo",
      "scope": "query-temperature",
      "expiresAt": "2025-06-18T13:51:20Z"
    }
  ]
}
```

The service operation **responds** with `201` if called successfully and tokens have been generated. The response also contains an [AuthorizationTokenListMgmtResponse](../data-models/authorization-token-list-mgmt-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "tokenType": "TIME_LIMITED_TOKEN",
      "variant": "TIME_LIMITED_TOKEN_AUTH",
      "token": "dsalefb521vdjkdsae633",
      "tokenReference": "a4626c853f0c0c8989757bb0ecc7b992",
      "requester": "TemperatureManager",
      "consumerCloud": "LOCAL",
      "consumer": "TemperatureConsumer",
      "provider": "TemperatureProvider1",
      "targetType": "SERVICE_DEF",
      "target": "kelvinInfo",
      "scope": "query-temperature",
      "createdAt": "2025-06-18T13:40:20Z",
      "expiresAt": "2025-06-18T13:51:20Z"
    }
  ],
  "count": 1
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":"Token variant is missing",
  "errorCode":400,
  "exceptionType": "INVALID_PARAMETER",
  "origin":"POST /consumerauthorization/authorization/mgmt/token/generate"
}
```

### query-tokens

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationTokenQueryRequest](../data-models/authorization-token-query-request.md) JSON encoded body.

```
POST /consumerauthorization/authorization/mgmt/token/query HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "pagination": {
    "page": 0,
    "size": 10
  },
  "requester": "TemperatureManager",
  "tokenType": "TIME_LIMITED_TOKEN",
  "consumerCloud": "LOCAL",
  "consumer": "TemperatureConsumer",
  "provider": "TemperatureProvider",
  "targetType": "SERVICE_DEF",
  "target": "kelvinInfo"
}
```

The service operation **responds** with `200` if called successfully and tokens have been queried. The response also contains an [AuthorizationTokenListMgmtResponse](../data-models/authorization-token-list-mgmt-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "tokenType": "TIME_LIMITED_TOKEN",
      "variant": "TIME_LIMITED_TOKEN_AUTH",
      "token": "dsalefb521vdjkdsae633",
      "tokenReference": "a4626c853f0c0c8989757bb0ecc7b992",
      "requester": "TemperatureManager",
      "consumerCloud": "LOCAL",
      "consumer": "TemperatureConsumer",
      "provider": "TemperatureProvider1",
      "targetType": "SERVICE_DEF",
      "target": "kelvinInfo",
      "scope": "query-temperature",
      "createdAt": "2025-06-18T13:40:20Z",
      "expiresAt": "2025-06-18T13:51:20Z"
    }
  ],
  "count": 1
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":"Invalid token type: SOMETHING",
  "errorCode":400,
  "exceptionType": "INVALID_PARAMETER",
  "origin":"POST /consumerauthorization/authorization/mgmt/token/query"
}
```

### revoke-tokens

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a query parameter _tokenReferences_, which is a List<[String](../primitives.md#string)>. It contains the references associated with the token records to be removed.

```
DELETE /consumerauthorization/authorization/mgmt/token/revoke?tokenReferences=a4626c853f0c0c8989757bb0ecc7b992&tokenReferences=a573c531f0b0b8789757bb0ecc5b882 HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":"No authentication info has been provided",
  "errorCode":401,
  "exceptionType":"AUTH",
  "origin":"DELETE /consumerauthorization/authorization/mgmt/token/revoke"
}
```

### add-encryption-keys

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationEncryptionKeyListRequest](../data-models/authorization-encryption-key-list-request.md) JSON encoded body.

```
POST /consumerauthorization/authorization/mgmt/token/encryption-key HTTP/1.1
Authorization: Bearer <identity-info>

{
  "list": [
    {
      "systemName": "TemperatureProvider2",
      "key": "abc1234",
      "algorithm": "AES/ECB/PKCS5Padding"
    }
  ]
}
```

The service operation **responds** with `201` if called successfully and the encryption key has been saved. The response also contains an [AuthorizationEncryptionKeyListResponse](../data-models/authorization-encryption-key-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "systemName": "TemperatureProvider2",
      "rawKey": "abc1234",
      "algorithm": "AES/ECB/PKCS5Padding",
      "keyAdditive": "",
      "createdAt": "2025-06-18T13:51:20Z"
    }
  ],
  "count": 1
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":"Unsupported algorithm",
  "errorCode":400,
  "exceptionType": "INVALID_PARAMETER",
  "origin":"POST /consumerauthorization/authorization/mgmt/token/encryption-key"
}
```

### remove-encryption-keys

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a query parameter _systemNames_, which is a List<[SystemName](../primitives.md#systemname)>. It contains the names of the systems to which the keys to be deleted belong.

```
DELETE /consumerauthorization/authorization/mgmt/token/encryption-key?systemNames=TemperatureProvider1&systemNames=TemperatureProvider2 HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage":"No authentication info has been provided",
  "errorCode":401,
  "exceptionType":"AUTH",
  "origin":"DELETE /consumerauthorization/authorization/mgmt/token/encryption-key"
}
```