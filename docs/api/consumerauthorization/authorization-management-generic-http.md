# authorizationManagement IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of authorizationManagement, which enables systems (with operator role or proper permissions) to handle (grant, revoke, query, check) authorization policies in bulk. An example of this interaction is when an operator uses the Management Tool to set up authorization policies manually before the related systems even register themselves. To enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry. 

Hereby the **Interface Design Description** (IDD) is provided to the [authorizationManagement – Service Description](../../assets/sd/5_0_0/authorization-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### grant-policies

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationMgmtGrantListRequest](../data-models/authorization-mgmt-grant-list-request.md)
JSON encoded body.

```
POST /consumerauthorization/authorization/mgmt/grant HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "list": [
    {
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
      }
    }
  ]
}
```

The service operation **responds** with 201 if called successfully. The response also contains an
[AuthorizationPolicyListResponse](../data-models/authorization-policy-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "instanceId": "MGMT|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo",
      "level": "MGMT",
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
      "createdBy": "Sysop",
      "createdAt": "2025-06-23T08:35:43.217717900Z"
    }
  ],
  "count": 1
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
  "origin": "POST /consumerauthorization/authorization/mgmt/grant"
}
```

### revoke-policies

The service operation **request**  requires an [identity related header or certificate](../authentication_policy.md/#http), and a List<[AuthorizationPolicyInstanceID](../primitives.md#authorizationpolicyinstanceid)> as query parameter using the key _instanceIds_, which contains the unique identifiers of the policy instances to be deleted.

```
DELETE /consumerauthorization/authorization/mgmt/revoke?instanceIds=MGMT%7CLOCAL%7CTemperatureProvider%7CSERVICE_DEF%7CcelsiusInfo HTTP1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Instance id list is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "DELETE /consumerauthorization/authorization/mgmt/revoke"
}
```

### query-policies

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationQueryRequest](../data-models/authorization-query-request.md) JSON encoded body.

```
POST /consumerauthorization/authorization/mgmt/query HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "pagination": {
    "page": 0,
    "size": 10
  },
  "level": "MGMT",
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
      "instanceId": "MGMT|LOCAL|TemperatureProvider2|SERVICE_DEF|kelvinInfo",
      "level": "MGMT",
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
      "createdBy": "Sysop",
      "createdAt": "2025-06-23T08:35:43Z"
    }
  ],
  "count": 1
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Level is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /consumerauthorization/authorization/mgmt/query"
}
```

### check-policies

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [AuthorizationMgmtVerifyListRequest](../data-models/authorization-mgmt-verify-list-request.md) JSON encoded body.

```
POST /consumerauthorization/authorization/mgmt/check HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "list": [
    {
      "provider": "TemperatureProvider2",
      "consumer": "TemperatureManager",
      "targetType": "SERVICE_DEF",
      "target": "kelvinInfo",
      "scope": "config"
    }
  ]
}
```

The service operation **responds** with the status code `200` if called successfully and with an [AuthorizationMgmtVerifyListResponse](../data-models/authorization-mgmt-verify-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "provider": "TemperatureProvider2",
      "consumer": "TemperatureManager",
      "cloud": "LOCAL",
      "targetType": "SERVICE_DEF",
      "target": "kelvinInfo",
      "scope": "config",
      "granted": true
    }
  ],
  "count": 1
}
```

The error codes are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` Error if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Provider is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /consumerauthorization/authorization/mgmt/check"
}
```