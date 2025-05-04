# service-registry-management IDD
**GENERIC-MQTT & GENERIC-MQTTS** 

## Overview

This page describes the GENERIC-MQTT and GENERIC-MQTTS service interface of the service-registry-management service, which enables systems (with operator role or proper permissions) to handle (register, update, revoke, lookup) [devices](#device-query), [systems](#system-query), [service instances](#service-query), [service definitions](#service-definition-query) and [interface templates](#interface-template-query) in bulk. An example of this interaction is that an operator uses the Management Tool to register interface templates, systems, and service instances manually. The interfaces are implemented using protocol, encoding as stated in the following tables:

**GENERIC-MQTT**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**GENERIC-MQTTS**

Profile type | type | Version
--- | --- | ---
Transfer protocol | MQTT | 3.1 and 3.1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [service-registry-management – Service Description](../../assets/sd/5_0_0/service-registry-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### device-query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [DeviceQueryRequest](../data-models/device-query-request.md).

```
Topic: arrowhead/serviceregistry/management/device-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "pagination": {
      "page": 0,
      "size": 10,
      "direction": "ASC",
      "sortField": "name"
    },
    "deviceNames": [
    ],
    "addresses": [
    ],
    "addressType": "",
    "metadataRequirementList": [
      {
        "volume.value": {"op": "GREATER_THAN_OR_EQUALS_TO", "value": 90},
        "volume.unit": {"op": "EQUALS", "value": "dB"}
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [DeviceListResponse](../data-models/device-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": {
    "entries": [
      {
        "name": "alarm1",
        "metadata": {
          "volume": {
            "value": 100,
            "unit": "dB"
          }
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "4a:f7:9c:12:8e:b5"
          }
        ],
        "createdAt": "2025-05-04T18:51:46Z",
        "updatedAt": "2025-05-04T18:51:46Z"
      },
      {
        "name": "alarm2",
        "metadata": {
          "volume": {
            "value": 110,
            "unit": "dB"
          }
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "4a:f7:9c:12:8e:bb"
          }
        ],
        "createdAt": "2025-05-04T18:51:46Z",
        "updatedAt": "2025-05-04T18:51:46Z"
      }
    ],
    "count": 2
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": "Direction is invalid. Only ASC or DESC are allowed"
}
```

### device-create

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [DeviceListRequest](../data-models/device-list-request.md).

```
Topic: arrowhead/serviceregistry/management/device-create

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "devices": [
      {
        "name": "alarm1",
        "metadata": {
          "volume": { "value": 100, "unit": "dB"}
        },
        "addresses": [
          "3a:f7:9c:12:8e:b5"
        ]
      },
      {
        "name": "alarm2",
        "metadata": {
          "volume": { "value": 110, "unit": "dB"}
        },
        "addresses": [
          "3a:f7:9c:12:8e:bb"
        ]
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if the device entities were successfully created. The response template payload is a [DeviceListResponse](../data-models/device-list-response.md).

```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": {
    "entries": [
      {
        "name": "alarm1",
        "metadata": {
          "volume": {
            "value": 100,
            "unit": "dB"
          }
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "3a:f7:9c:12:8e:b5"
          }
        ],
        "createdAt": "2025-05-04T18:51:46.399919700Z",
        "updatedAt": "2025-05-04T18:51:46.399919700Z"
      },
      {
        "name": "alarm2",
        "metadata": {
          "volume": {
            "value": 110,
            "unit": "dB"
          }
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "3a:f7:9c:12:8e:bb"
          }
        ],
        "createdAt": "2025-05-04T18:51:46.409974400Z",
        "updatedAt": "2025-05-04T18:51:46.409974400Z"
      }
    ],
    "count": 2
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": "Device with names already exists: alarm1, alarm2"
}
```

### device-update

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [DeviceListRequest](../data-models/device-list-request.md).

```
Topic: arrowhead/serviceregistry/management/device-update

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "devices": [
      {
        "name": "alarm1",
        "metadata": {
          "volume": { "value": 100, "unit": "dB"}
        },
        "addresses": [
          "4a:f7:9c:12:8e:b5"
        ]
      },
      {
        "name": "alarm2",
        "metadata": {
          "volume": { "value": 110, "unit": "dB"}
        },
        "addresses": [
          "4a:f7:9c:12:8e:bb"
        ]
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [DeviceListResponse](../data-models/device-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": {
    "entries": [
      {
        "name": "alarm1",
        "metadata": {
          "volume": {
            "value": 100,
            "unit": "dB"
          }
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "4a:f7:9c:12:8e:b5"
          }
        ],
        "createdAt": "2025-05-04T18:51:46Z",
        "updatedAt": "2025-05-04T18:51:46Z"
      },
      {
        "name": "alarm2",
        "metadata": {
          "volume": {
            "value": 110,
            "unit": "dB"
          }
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "4a:f7:9c:12:8e:bb"
          }
        ],
        "createdAt": "2025-05-04T18:51:46Z",
        "updatedAt": "2025-05-04T18:51:46Z"
      }
    ],
    "count": 2
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": "Device(s) not exists: alarm-1, alarm-2"
}
```

### device-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[Name](../primitives.md#name)> as path parameter, which contains the names of the devices to delete.

```
Topic: arrowhead/serviceregistry/management/device-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": ["alarm1", "alarm2"]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case, the response payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `423` if entity is not removable and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 423,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": "At least one system is assigned to these devices."
}
```

### system-query