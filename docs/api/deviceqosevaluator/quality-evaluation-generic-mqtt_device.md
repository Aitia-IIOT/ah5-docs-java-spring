# qualityEvaluation IDD (basic-device-kpi)
**generic_mqtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of the `basic-device-kpi` type of qualityEvaluation, which enables to evaluate provider systems based on their device level KPIs.

Hereby the **Interface Design Description** (IDD) is provided to the [qualityEvaluation – Service Description](../../assets/sd/5_2_0/qualityEvaluation_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### filter

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [QoSEvaluationRequest](../data-models/qos-evaluation-request.md) JSON encoded body in which the `configuration` is a [QoSDeviceDataEvaluationConfig](../data-models/qos-device-data-evaluation-request.md).

```
Topic: arrowhead/deviceqosevaluator/qualityevaluation/filter

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic":" <response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
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
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [QoSEvaluationFilterResponse](../data-models/qos-evaluation-filter-response.md) JSON encoded body.

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TestConsumer",
   "payload": {
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
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TestConsumer",
   "payload": {
      "errorMessage": "Configuration payload is missing",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/deviceqosevaluator/qualityevaluation/filter"
   }
}
```

### sort

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [QoSEvaluationRequest](../data-models/qos-evaluation-request.md) JSON encoded body in which the `configuration` is a [QoSDeviceDataEvaluationConfig](../data-models/qos-device-data-evaluation-request.md).

```
Topic: arrowhead/deviceqosevaluator/qualityevaluation/sort

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic":" <response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
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
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [QoSEvaluationSortResponse](../data-models/qos-evaluation-sort-response.md) JSON encoded body.

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TestConsumer",
   "payload": {
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
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TestConsumer",
   "payload": {
      "errorMessage": "Configuration payload is missing",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/deviceqosevaluator/qualityevaluation/sort"
   }
}
```