# qualityEvaluation IDD (basic-device-kpi)
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of the `basic-device-kpi` type of qualityEvaluation, which enables to evaluate provider systems based on their device level KPIs.

Hereby the **Interface Design Description** (IDD) is provided to the [qualityEvaluation – Service Description](../../assets/sd/5_2_0/qualityEvaluation_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### filter

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [QoSEvaluationRequest](../data-models/qos-evaluation-request.md) JSON encoded body in which the `configuration` is a [QoSDeviceDataEvaluationConfig](../data-models/qos-device-data-evaluation-request.md).

```
POST /deviceqosevaluator/qualityevaluation/filter HTTP/1.1
Authorization: Bearer <identity-info>

{
   "providers": [
      "ProviderSysA",
      "ProviderSysB",
      "ProviderSysC",
      "ProviderSysD"
   ],
   "configuration": {
      "metricNames": [
         "RTT_MAXIMUM",
         "CPU_TOTAL_LOAD_MEAN",
         "MEMORY_USED_MEDIAN",
         "NETWORK_EGRESS_LOAD_CURRENT"
      ],
      "metricWeights": [
         0.3,
         0.4,
         0.15,
         0.15
      ],
      "timeWindow": 90,
      "threshold": 45.5
   }
}
```

The service operation **responds** with the status code `200` if called successfully and with a [QoSEvaluationFilterResponse](../data-models/qos-evaluation-filter-response.md) JSON encoded body.

```
{
   "passedProviders": [
      "ProviderSysB",
      "ProviderSysC"
   ],
   "droppedProviders": [
      "ProviderSysA",
      "ProviderSysD"
   ],
   "warnings": {
      "ProviderSysD": [
         "CPU_TOTAL_LOAD",
         "MEMORY_USED"
      ]
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Configuration payload is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /deviceqosevaluator/qualityevaluation/filter"
}
```

### sort

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [QoSEvaluationRequest](../data-models/qos-evaluation-request.md) JSON encoded body in which the `configuration` is a [QoSDeviceDataEvaluationConfig](../data-models/qos-device-data-evaluation-request.md).

```
POST /deviceqosevaluator/qualityevaluation/sort HTTP/1.1
Authorization: Bearer <identity-info>

{
   "providers": [
      "ProviderSysA",
      "ProviderSysB",
      "ProviderSysC",
      "ProviderSysD"
   ],
   "configuration": {
      "metricNames": [
         "RTT_MAXIMUM",
         "CPU_TOTAL_LOAD_MEAN",
         "MEMORY_USED_MEDIAN",
         "NETWORK_EGRESS_LOAD_CURRENT"
      ],
      "metricWeights": [
         0.3,
         0.4,
         0.15,
         0.15
      ],
      "timeWindow": 90
   }
}
```

The service operation **responds** with the status code `200` if called successfully and with a [QoSEvaluationSortResponse](../data-models/qos-evaluation-sort-response.md) JSON encoded body.

```
{
   "sortedProviders": [
      "ProviderSysB",
      "ProviderSysC",
      "ProviderSysA",
      "ProviderSysD"
   ],
   "warnings": {
      "ProviderSysD": [
         "CPU_TOTAL_LOAD",
         "MEMORY_USED"
      ]
   }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Configuration payload is missing",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /deviceqosevaluator/qualityevaluation/sort"
}
```