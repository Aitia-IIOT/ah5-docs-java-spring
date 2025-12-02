# deviceQualityDataManagement IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of deviceQualityDataManagement, which enables to query the measurement metric values and also to trigger the synchronization with ServiceRegistry Core System manually.

Hereby the **Interface Design Description** (IDD) is provided to the [deviceQualityDataManagement – Service Description](../../assets/sd/5_2_0/deviceQualityDataManagement_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [QoSDeviceStatQueryRequest](../data-models/qos-device-stat-query-request.md) JSON encoded body.

```
POST /deviceqosevaluator/mgmt/query HTTP/1.1
Authorization: Bearer <identity-info>

{
   "pagination": {
      "page": 0,
      "size": 10,
      "direction": "ASC",
      "sortField": "timestamp"
   },
   "metricGroup": "CPU_TOTAL_LOAD",
   "from": "2025-11-08T11:24:00Z",
   "to": "2025-11-08T11:30:00Z",
   "aggregation": [
      "MEAN",
      "MAXIMUM"
   ],
   "systemNames": [
      "ProviderSysA",
      "ProviderSysB"
   ]
}
```

The service operation **responds** with the status code `200` if called successfully and with a [QoSDeviceStatQueryResponse](../data-models/qos-device-stat-query-response.md) JSON encoded body.

```
{
   "count": 10,
   "records": [
      {
         "metricGroup": "CPU_TOTAL_LOAD",
         "id": 1235,
         "timestamp": "2025-11-08T11:24:43Z",
         "uuid": "18c0234e-3f16-47c1-82e0-c7bac2a1d2c4",
         "mean": 15.3,
         "maximum": 16.2,
         "systems": [
            "ProviderSysA",
            "ProviderSysB"
         ]
      }
   ]
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "metricGroup is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /deviceqosevaluator/mgmt/query"
}
```

### reload

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http).

```
POST /deviceqosevaluator/mgmt/reload HTTP/1.1
Authorization: Bearer <identity-info>
```

The service operation **responds** with the status code `200` if called successfully and with a `Reload operation is already in proggress` or `<x> more systems found, <y> systems removed` plain text message.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Database operation error",
  "errorCode": 500,
  "exceptionType": "INTERNAL_SERVER_ERROR",
  "origin": "POST /deviceqosevaluator/mgmt/reload"
}
```