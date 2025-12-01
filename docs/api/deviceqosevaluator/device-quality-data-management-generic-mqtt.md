# deviceQualityDataManagement IDD
**generic_mqtt & generic_mqtts**

## Overview

This page describes the [generic_mqtt](../communication-profiles/generic-mqtt-template.md) and [generic_mqtts](../communication-profiles/generic-mqtts-template.md) service interface of deviceQualityDataManagement, which enables to query the measurement metric values and also to trigger the synchronization with ServiceRegistry Core System manually.

Hereby the **Interface Design Description** (IDD) is provided to the [deviceQualityDataManagement – Service Description](../../assets/sd/5_2_0/deviceQualityDataManagement_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [QoSDeviceStatQueryRequest](../data-models/qos-device-stat-query-request.md). 

```
Topic: arrowhead/deviceqosevaluator/management/query

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "payload": {
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
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [QoSDeviceStatQueryResponse](../data-models/qos-device-stat-query-response.md).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TestConsumer",
   "payload": {
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
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 400,
   "traceId": "<trace-id>",
   "receiver": "TestConsumer",
   "payload": {
      "errorMessage": "metricGroup is missing",
      "errorCode": 400,
      "exceptionType": "INVALID_PARAMETER",
      "origin": "arrowhead/deviceqosevaluator/management/query"
   }
}
```

### reload

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt). 

```
Topic: arrowhead/deviceqosevaluator/management/reload

{
   "traceId": "<trace-id>",
   "authentication": "<authentication-data>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a simple [String](../primitives.md#string).

```
{
   "status": 200,
   "traceId": "<trace-id>",
   "receiver": "TestConsumer",
   "payload": "<x> more systems found, <y> systems removed"
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `500` if an unexpected error happens and `503` if an unexpected error happens while communicating with other systems. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
   "status": 500,
   "traceId": "<trace-id>",
   "receiver": "TestConsumer",
   "payload": {
      "errorMessage": "Database operation error",
      "errorCode": 500,
      "exceptionType": "INTERNAL_SERVER_ERROR",
      "origin": "arrowhead/deviceqosevaluator/management/reload"
   }
}
```