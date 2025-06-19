# authorization IDD
**generic_http & generic_https**

## Overview

This page describes the generic_http and generic_https service interface of authorization, which enables service consumption permission validations
for both providers and consumers. Additionally, providers can lookup, grant and revoke those permissions. An example of this interaction when a provider system creates authorization policies about its offered service. An
other example when a consumer can check whether a service is allowed to use before trying an actual service consumption. Event notification permission is also handled by this service in an event publisher/subscriber
scenario. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry. 

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

Hereby the **Interface Design Description** (IDD) is provided to the [authorization – Service Description](../../assets/sd/5_0_0/authorization_sd.pdf). For further details about how this service is meant to be used, please consult that document.

### grant

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationGrantRequest](../data-models/authorization-grant-request.md)
JSON encoded body.

```
POST /consumerauthorization/authorization/grant HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "targetType": "SERVICE_DEF",
  "target": "kelvinInfo",
  "description": "query for everyone, config for TemperatureManager only",
  "defaultPolicy": {
    "policyType": "ALL"
  },
  "scopedPolicies": {
    "config": {
      "policyType": "WHITELIST",
      "policyList": [
        "TemperatureManager"
      ]
    }
  }
}
```

The service operation **responds** with 200 if called successfully and the policy instance is already existing or 201 if the entity was newly created. The response also contains an
[AuthorizationResponse](../data-models/authorization-policy-response.md) JSON encoded body.

```
{
  "instanceId": "PR|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo",
  "level": "PROVIDER",
  "cloud": "LOCAL",
  "provider": "TemperatureProvider2",
  "targetType": "SERVICE_DEF",
  "target": "kelvinInfo",
  "description": "query for everyone, config for TemperatureManager only",
  "defaultPolicy": {
    "policyType": "ALL"
  },
  "scopedPolicies": {
    "config": {
      "policyType": "WHITELIST",
      "policyList": [
        "TemperatureManager"
      ]
    }
  },
  "createdBy": "TemperatureProvider2",
  "createdAt": "2025-06-18T13:51:19.727425900Z"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Target is missing"",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /consumerauthorization/authorization/grant"
}
```

### revoke

The service operation **request**  requires an [identity related header or certificate](../authentication_policy.md/#http), and an [AuthorizationPolicyInstanceID](../primitives.md#authorizationpolicyinstanceid) as path parameter, which is a unique identifier of the policy instance to be deleted.

```
DELETE /consumerauthorization/authorization/revoke/PR%7CLOCAL%7CTemperatureProvider%7CSERVICE_DEF%7CcelsiusInfo HTTP1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with the status code `200` if called successfully and an existing policy instance entity was removed and `204` if no matching entity was found. The success response does not contain any response body.

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Revoking other systems' policy is forbidden",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN",
  "origin": "DELETE /consumerauthorization/authorization/revoke/PR|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo"
}
```

### lookup

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationLookupRequest](../data-models/authorization-lookup-request.md) JSON encoded body.

```
POST /consumerauthorization/authorization/lookup HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "instanceIds": [
  ],
  "cloudIdentifiers": [
  ],
  "targetNames": [
    "kelvinInfo"
  ],
  "targetType": "SERVICE_DEF"
}
```

The service operation **responds** with the status code `200` if called successfully and with an [AuthorizationPolicyListResponse](../data-models/authorization-policy-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "instanceId": "PR|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo",
      "level": "PROVIDER",
      "cloud": "LOCAL",
      "provider": "TemperatureProvider2",
      "targetType": "SERVICE_DEF",
      "target": "kelvinInfo",
      "description": "query for everyone, config for TemperatureManager only",
      "defaultPolicy": {
        "policyType": "ALL"
      },
      "scopedPolicies": {
        "config": {
          "policyType": "WHITELIST",
          "policyList": [
            "TemperatureManager"
          ]
        }
      },
      "createdBy": "TemperatureProvider2",
      "createdAt": "2025-06-18T13:51:20Z"
    }
  ],
  "count": 1
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "One of the following filters must be used: 'instanceIds', 'targetNames', 'cloudIdentifiers'",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /consumerauthorization/authorization/lookup"
}
```

### verify

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationVerifyRequest](../data-models/authorization-verify-request.md) JSON encoded body.

```
POST /consumerauthorization/authorization/verify HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "provider": "TemperatureProvider2",
  "consumer": "TemperatureManager",
  "targetType": "SERVICE_DEF",
  "target": "kelvinInfo",
  "scope": "config"
}
```

The service operation **responds** with the status code `200` if called successfully and with a [Boolean](../primitives.md#boolean) JSON encoded body.

```
true
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Only the related provider or consumer can use this operation",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN",
  "origin": "POST /consumerauthorization/authorization/verify"
}
```