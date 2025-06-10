# serviceRegistryManagement IDD

**generic_mqtt & generic_mqtts** 

## Overview

This page describes the generic_mqtt and generic_mqtts service interface of the serviceRegistryManagement service, which enables systems (with operator role or proper permissions) to handle (register, update, revoke, lookup) [devices](#device-query), [systems](#system-query), [service instances](#service-query), [service definitions](#service-definition-query) and [interface templates](#interface-template-query) in bulk. An example of this interaction is that an operator uses the Management Tool to register interface templates, systems, and service instances manually. The interfaces are implemented using protocol, encoding as stated in the following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [serviceRegistryManagement – Service Description](../../assets/sd/5_0_0/service-registry-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### device-query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an optional [DeviceQueryRequest](../data-models/device-query-request.md).

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
        "volume.value": { "op": "GREATER_THAN_OR_EQUALS_TO", "value": 90 },
        "volume.unit": { "op": "EQUALS", "value": "dB" }
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "ALARM1",
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
        "name": "ALARTM2",
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Sort field is invalid. Only the following are allowed: [id, name, createdAt]",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/device-query"
  }
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
        "name": "ALARM1",
        "metadata": {
          "volume": { "value": 100, "unit": "dB" }
        },
        "addresses": [
          "3a:f7:9c:12:8e:b5"
        ]
      },
      {
        "name": "ALARM2",
        "metadata": {
          "volume": { "value": 110, "unit": "dB" }
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "ALARM1",
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
        "name": "ALARM2",
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Device with names already exists: ALARM1, ALARM2",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/device-create"
  }
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
        "name": "ALARM1",
        "metadata": {
          "volume": { "value": 100, "unit": "dB" }
        },
        "addresses": [
          "4a:f7:9c:12:8e:b5"
        ]
      },
      {
        "name": "ALARM2",
        "metadata": {
          "volume": { "value": 110, "unit": "dB" }
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "ALARM1",
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
        "name": "ALARM2",
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Device(s) not exists: ALARM001, ALARM002",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/device-update"
  }
}
```

### device-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[DeviceName](../primitives.md#devicename)> which contains the names of the devices to delete.

```
Topic: arrowhead/serviceregistry/management/device-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": ["ALARM1", "ALARM2"]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case the response payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `423` if the entity is not removable and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 423,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "At least one system is assigned to these devices",
    "errorCode": 423,
    "exceptionType": "LOCKED",
    "origin": "arrowhead/serviceregistry/management/device-remove"
  }
}
```

### system-query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an optional [SystemQueryRequest](../data-models/system-query-request.md). The params can contain a [KeyValuePair](../primitives.md/#keyvaluepair) with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device information also returns (only if the provider supports it).

```
Topic: arrowhead/serviceregistry/management/system-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "params": {"verbose": false},
  "payload": {
    "pagination": {
      "page": 1,
      "size": 1,
      "direction": "ASC",
      "sortField": ""
    },
    "systemNames": [
    ],
    "addresses": [
    ],
    "addressType": "",
    "metadataRequirementList": [
    ],
    "versions": [
      "1.1"
    ],
    "deviceNames": [
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [SystemListResponse](../data-models/system-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "AlertConsumer2",
        "metadata": {},
        "version": "1.1.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          }
        ],
        "createdAt": "2025-03-15T20:22:44Z",
        "updatedAt": "2025-03-15T20:22:44Z"
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "The page size cannot be larger than 1000",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/system-query"
  }
}
```

### system-create

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [SystemListRequest](../data-models/system-list-request.md).

```
Topic: arrowhead/serviceregistry/management/system-create

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "systems": [
      {
        "name": "AlertConsumer1",
        "metadata": {
        },
        "version": "1.1",
        "addresses": [
          "192.168.1.1"
        ],
        "deviceName": "ALARM1"
      },
      {
        "name": "AlertConsumer2",
        "metadata": {
        },
        "version": "1.1",
        "addresses": [
          "192.168.1.2"
        ],
        "deviceName": "ALARM2"
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if the system entities were successfully created. The response template payload is a [SystemListResponse](../data-models/system-list-response.md).

```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "AlertConsumer1",
        "metadata": {},
        "version": "1.1.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.1"
          }
        ],
        "device": {
          "name": "ALARM1",
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
        "createdAt": "2025-05-04T20:08:37.278728600Z",
        "updatedAt": "2025-05-04T20:08:37.278728600Z"
      },
      {
        "name": "AlertConsumer2",
        "metadata": {},
        "version": "1.1.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          }
        ],
        "device": {
          "name": "ALARM2",
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
        },
        "createdAt": "2025-05-04T20:08:37.284276300Z",
        "updatedAt": "2025-05-04T20:08:37.284276300Z"
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Systems with names already exist: AlertConsumer1, AlertConsumer2",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/system-create"
  }
}
```

### system-update

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [SystemListRequest](../data-models/system-list-request.md).

```
Topic: arrowhead/serviceregistry/management/system-update

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "systems": [
      {
        "name": "AlertConsumer1",
        "metadata": {
        },
        "version": "1.2",
        "addresses": [
          "192.168.1.1"
        ],
        "deviceName": "ALARM1"
      },
      {
        "name": "AlertConsumer2",
        "metadata": {
        },
        "version": "1.2",
        "addresses": [
          "192.168.1.2"
        ],
        "deviceName": "ALARM2"
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [SystemListResponse](../data-models/system-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "AlertConsumer1",
        "metadata": {},
        "version": "1.2.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.1"
          }
        ],
        "device": {
          "name": "ALARM1",
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
        "createdAt": "2025-05-04T20:08:37Z",
        "updatedAt": "2025-05-04T20:11:20.541557100Z"
      },
      {
        "name": "AlertConsumer2",
        "metadata": {},
        "version": "1.2.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          }
        ],
        "device": {
          "name": "ALARM2",
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
        },
        "createdAt": "2025-05-04T20:08:37Z",
        "updatedAt": "2025-05-04T20:11:20.661848200Z"
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Duplicated system name: AlertConsumer1",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/system-update"
  }
}
```

### system-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[SystemName](../primitives.md#systemname)> which contains the names of the systems to delete.

```
arrowhead/serviceregistry/management/system-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": ["AlertConsumer1", "AlertConsumer2"]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case the response payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 401,
  "traceId": "<trace-id>",
  "receiver": null,
  "payload": {
    "errorMessage": "Invalid authentication info",
    "errorCode": 401,
    "exceptionType": "AUTH"
  }
}
```

### service-definition-query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an optional [PageRequest](../data-models/page-request.md).

```
Topic: arrowhead/serviceregistry/management/service-definition-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "page": 2,
    "size": 4,
    "direction": "DESC",
    "sortField": "name"
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [ServiceDefinitionListResponse](../data-models/service-definition-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "orchestrationLockManagement",
        "createdAt": "2025-03-12T11:07:23Z",
        "updatedAt": "2025-03-12T11:07:23Z"
      },
      {
        "name": "orchestrationHistoryManagement",
        "createdAt": "2025-03-12T11:07:23Z",
        "updatedAt": "2025-03-12T11:07:23Z"
      },
      {
        "name": "orchestration",
        "createdAt": "2025-03-12T11:07:23Z",
        "updatedAt": "2025-03-12T11:07:23Z"
      },
      {
        "name": "monitor",
        "createdAt": "2025-01-31T09:14:54Z",
        "updatedAt": "2025-01-31T09:14:54Z"
      }
    ],
    "count": 26
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Direction is invalid. Only ASC or DESC are allowed",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/service-definition-query"
  }
}
```

### service-definition-create 

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceDefinitionListRequest](../data-models/service-definition-list-request.md).

```
Topic: arrowhead/serviceregistry/management/service-definition-create

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "serviceDefinitionNames": [
      "alertService1", "alertService2"
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [ServiceDefinitionListResponse](../data-models/service-definition-list-response.md).

```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "alertService1",
        "createdAt": "2025-05-04T21:33:49.344720800Z",
        "updatedAt": "2025-05-04T21:33:49.344720800Z"
      },
      {
        "name": "alertService2",
        "createdAt": "2025-05-04T21:33:49.421567600Z",
        "updatedAt": "2025-05-04T21:33:49.421567600Z"
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Service definition names already exists: alertService1",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/service-definition-create"
  }
}
```

### service-definition-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[ServiceName](../primitives.md#servicename)> which contains the names of the service definitions to delete.

```
Topic: arrowhead/serviceregistry/management/service-definition-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": ["alertService1", "alertService2"]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case the response payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Service definition name list is missing or empty",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/service-definition-remove"
  }
}
```

### service-query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceQueryRequest](../data-models/service-query-request.md). The params can contain a [KeyValuePair](../primitives.md/#keyvaluepair) with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device information also returns (only if the provider supports it).

```
Topic: arrowhead/serviceregistry/management/service-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "params": {"verbose": false},
  "payload": {
    "pagination": {
      "page": 0,
      "size": 2,
      "direction": "ASC",
      "sortField": "createdAt"
    },
    "instanceIds": [
    ],
    "providerNames": [
    ],
    "serviceDefinitionNames": [
      "alertService1"
    ],
    "versions": [
      "1.0.0", "1.0.1"
    ],
    "alivesAt": "2026-01-01T00:00:00Z",
    "metadataRequirementsList": [
    ],
    "addressTypes": [
    ],
    "interfaceTemplateNames": [
      "generic_mqtt"
    ],
    "interfacePropertyRequirementsList": [
      {
        "operations": { "op": "CONTAINS", "value": "warn" }
      }
    ],
    "policies": [
      "NONE"
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [ServiceListResponse](../data-models/service-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "instanceId": "AlertProvider1|alertService1|1.0.0",
        "provider": {
          "name": "AlertProvider1",
          "metadata": {},
          "version": "1.1.0",
          "createdAt": "2025-05-05T09:05:31Z",
          "updatedAt": "2025-05-05T09:05:31Z"
        },
        "serviceDefinition": {
          "name": "alertService1",
          "createdAt": "2025-05-04T21:33:49Z",
          "updatedAt": "2025-05-04T21:33:49Z"
        },
        "version": "1.0.0",
        "expiresAt": "2028-01-01T00:00:00Z",
        "metadata": {
          "delay": {
            "value": 200,
            "unit": "ms"
          }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.3"
              ],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
              "operations": [
                "alert",
                "warn",
                "info"
              ]
            }
          }
        ],
        "createdAt": "2025-05-05T09:06:28Z",
        "updatedAt": "2025-05-05T09:06:28Z"
      }
    ],
    "count": 1
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Sort field is invalid. Only the following are allowed: [id, name, createdAt]",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/service-query"
  }
}
```

### service-create

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceCreateListRequest](../data-models/service-create-list-request.md).

```
Topic: arrowhead/serviceregistry/management/service-create

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "instances": [
      {
        "systemName": "AlertProvider1",
        "serviceDefinitionName": "alertService1",
        "version": "",
        "expiresAt": "2028-01-01T00:00:00Z",
        "metadata": {
          "delay": { "value": 200, "unit": "ms" }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp", 
            "policy": "NONE",
            "properties": {
              "accessAddresses": [ "192.168.1.3" ],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
			  "operations": [ "alert", "warn" ]
            }
          }
        ]
      },
      {
        "systemName": "AlertProvider2",
        "serviceDefinitionName": "alertService2",
        "version": "",
        "expiresAt": "2027-01-01T00:00:00Z",
        "metadata": {
          "delay": { "value": 200, "unit": "ms" }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp", 
            "policy": "NONE",
            "properties": {
              "accessAddresses": [ "192.168.1.4" ],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
			  "operations": [ "warn" ]
            }
          }
        ]
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if the service instance entities were created successfully. The response template payload is a [ServiceListResponse](../data-models/service-list-response.md).

```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "instanceId": "AlertProvider1|alertService1|1.0.0",
        "provider": {
          "name": "AlertProvider1",
          "metadata": {},
          "version": "1.1.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.1"
            }
          ],
          "device": {
            "name": "ALARM1",
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
          "createdAt": "2025-05-05T09:05:31Z",
          "updatedAt": "2025-05-05T09:05:31Z"
        },
        "serviceDefinition": {
          "name": "alertService1",
          "createdAt": "2025-05-04T21:33:49Z",
          "updatedAt": "2025-05-04T21:33:49Z"
        },
        "version": "1.0.0",
        "expiresAt": "2028-01-01T00:00:00Z",
        "metadata": {
          "delay": {
            "value": 200,
            "unit": "ms"
          }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.3"
              ],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
              "operations": [
                "alert",
                "warn"
              ]
            }
          }
        ],
        "createdAt": "2025-05-05T09:06:27.599446600Z",
        "updatedAt": "2025-05-05T09:06:27.599446600Z"
      },
      {
        "instanceId": "AlertProvider2|alertService1|1.0.0",
        "provider": {
          "name": "AlertProvider2",
          "metadata": {},
          "version": "1.1.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.2"
            }
          ],
          "device": {
            "name": "ALARM2",
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
          },
          "createdAt": "2025-05-05T09:05:32Z",
          "updatedAt": "2025-05-05T09:05:32Z"
        },
        "serviceDefinition": {
          "name": "alertService2",
          "createdAt": "2025-05-04T21:33:49Z",
          "updatedAt": "2025-05-04T21:33:49Z"
        },
        "version": "1.0.0",
        "expiresAt": "2027-01-01T00:00:00Z",
        "metadata": {
          "delay": {
            "value": 200,
            "unit": "ms"
          }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.4"
              ],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
              "operations": [
                "warn"
              ]
            }
          }
        ],
        "createdAt": "2025-05-05T09:06:27.618441200Z",
        "updatedAt": "2025-05-05T09:06:27.618441200Z"
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Service definition name is empty",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/service-create"
  }
}
```

### service-update

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceUpdateListRequest](../data-models/service-update-list-request.md).

```
Topic: arrowhead/serviceregistry/management/service-update

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "instances": [
      {
        "instanceId": "AlertProvider1|alertService1|1.0.0",
        "expiresAt": "2028-01-01T00:00:00Z",
        "metadata": {
          "delay": { "value": 200, "unit": "ms" }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp", 
			"policy": "NONE",
            "properties": {
              "accessAddresses": [ "192.168.1.3" ],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
			  "operations": [ "alert", "warn", "info" ]
            }
          }
        ]
      },
      {
        "instanceId": "AlertProvider2|alertService2|1.0.0",
        "expiresAt": "2027-01-01T00:00:00Z",
        "metadata": {
          "delay": { "value": 200, "unit": "ms" }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp",
ű			"policy": "NONE",
            "properties": {
              "accessAddresses": ["192.168.1.4"],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
			  "operations": [ "warn" ]
            }
          }
        ]
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if the service instance entities were updated successfully. The response template payload is a [ServiceListResponse](../data-models/service-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "instanceId": "AlertProvider1|alertService1|1.0.0",
        "provider": {
          "name": "AlertProvider1",
          "metadata": {},
          "version": "1.1.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.1"
            }
          ],
          "device": {
            "name": "ALARM1",
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
          "createdAt": "2025-05-05T09:05:31Z",
          "updatedAt": "2025-05-05T09:05:31Z"
        },
        "serviceDefinition": {
          "name": "alertService1",
          "createdAt": "2025-05-04T21:33:49Z",
          "updatedAt": "2025-05-04T21:33:49Z"
        },
        "version": "1.0.0",
        "expiresAt": "2028-01-01T00:00:00Z",
        "metadata": {
          "delay": {
            "value": 200,
            "unit": "ms"
          }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.3"
              ],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
              "operations": [
                "alert",
                "warn",
                "info"
              ]
            }
          }
        ],
        "createdAt": "2025-05-05T09:06:28Z",
        "updatedAt": "2025-05-05T09:09:58.070188100Z"
      },
      {
        "instanceId": "AlertProvider2|alertService2|1.0.0",
        "provider": {
          "name": "AlertProvider2",
          "metadata": {},
          "version": "1.1.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.2"
            }
          ],
          "device": {
            "name": "ALARM2",
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
          },
          "createdAt": "2025-05-05T09:05:32Z",
          "updatedAt": "2025-05-05T09:05:32Z"
        },
        "serviceDefinition": {
          "name": "alertService2",
          "createdAt": "2025-05-04T21:33:49Z",
          "updatedAt": "2025-05-04T21:33:49Z"
        },
        "version": "1.0.0",
        "expiresAt": "2027-01-01T00:00:00Z",
        "metadata": {
          "delay": {
            "value": 200,
            "unit": "ms"
          }
        },
        "interfaces": [
          {
            "templateName": "generic_mqtt",
            "protocol": "tcp",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.4"
              ],
              "accessPort": 1883,
              "baseTopic": "heat-alert",
              "operations": [
                "warn"
              ]
            }
          }
        ],
        "createdAt": "2025-05-05T09:06:28Z",
        "updatedAt": "2025-05-05T09:09:58.238440Z"
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": ""Instance id does not exist: AlertProvider1|alertService1|1.0.1"",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/service-update"
  }
}
```
### service-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[ServiceInstanceID](../primitives.md#serviceinstanceid)> which contains the identitifers of the service instances that need to be removed.

```
Topic: arrowhead/serviceregistry/management/service-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": ["AlertProvider1|alertService1|1.0.0", "AlertProvider2|alertService2|1.0.0"]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case the response payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": ""
}
```
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 403,
  "traceId": "<trace-id>",
  "receiver": <receiver-system-identifier>,
  "payload": {
    "errorMessage": "Requester has no management permission",
    "errorCode": 403,
    "exceptionType": "FORBIDDEN",
    "origin": "arrowhead/serviceregistry/management/service-remove"
  }
}
```

### interface-template-query

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an optional [InterfaceTemplateQueryRequest](../data-models/interface-template-query-request.md).

```
Topic: arrowhead/serviceregistry/management/interface-template-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "pagination": {
      "page": 1,
      "size": 1,
      "direction": "ASC",
      "sortField": "name"
    },
    "templateNames": [
    ],
    "protocols": [
      "tcp"
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [InterfaceTemplateListResponse](../data-models/interface-template-list-response.md).

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "generic_mqtt",
        "protocol": "tcp",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "NOT_EMPTY_ADDRESS_LIST",
            "validatorParams": []
          },
          {
            "name": "accessPort",
            "mandatory": true,
            "validator": "PORT",
            "validatorParams": []
          },
          {
            "name": "baseTopic",
            "mandatory": true
          },
          {
            "name": "operations",
            "mandatory": true,
            "validator": "NOT_EMPTY_STRING_SET",
            "validatorParams": [
              "OPERATION"
            ]
          }
        ],
        "createdAt": "2024-12-09T18:52:48Z",
        "updatedAt": "2024-12-09T18:52:48Z"
      }
    ],
    "count": 3
  }
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 400,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "The specified interface template name does not match the naming convention: general@mqtt",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/interface-template-query"
  }
}
```

### interface-template-create

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an [InterfaceTemplateListRequest](../data-models/interface-template-list-request.md).

```
Topic: arrowhead/serviceregistry/management/interface-template-create

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "interfaceTemplates": [
      {
        "name": "custom_ftp",
        "protocol": "tcp",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "not_empty_address_list",
            "validatorParams": [
            ]
          }
        ]
      },
      {
        "name": "my_awesome_ftp",
        "protocol": "tcp",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "not_empty_address_list",
            "validatorParams": [
            ]
          }
        ]
      }
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `201` if the interface template entities were successfully created. [InterfaceTemplateListResponse](../data-models/interface-template-list-response.md).

```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "entries": [
      {
        "name": "custom_ftp",
        "protocol": "tcp",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "NOT_EMPTY_ADDRESS_LIST",
            "validatorParams": []
          }
        ],
        "createdAt": "2025-05-05T10:26:35.426965600Z",
        "updatedAt": "2025-05-05T10:26:35.426965600Z"
      },
      {
        "name": "my_awesome_ftp",
        "protocol": "tcp",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "NOT_EMPTY_ADDRESS_LIST",
            "validatorParams": []
          }
        ],
        "createdAt": "2025-05-05T10:26:35.532789400Z",
        "updatedAt": "2025-05-05T10:26:35.532789400Z"
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
  "receiver": "<receiver-system-identifier>",
  "payload": {
    "errorMessage": "Interface template already exists: custom_ftp",
    "errorCode": 400,
    "exceptionType": "INVALID_PARAMETER",
    "origin": "arrowhead/serviceregistry/management/interface-template-create"
  }
}
```

### interface-template-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[InterfaceName](../primitives.md#interfacename)> which contains the string identifier of the interface descriptors that need to be removed.

```
Topic: arrowhead/serviceregistry/management/interface-template-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": [ "custom_ftp", "my_awesome_ftp" ]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case the response payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "<receiver-system-identifier>",
  "payload": ""
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. In these cases the response template payload is an [ErrorResponse](../data-models/error-response.md) JSON.

```
{
  "status": 401,
  "traceId": "<trace-id>",
  "receiver": null,
  "payload": "Invalid authentication info"
}
```