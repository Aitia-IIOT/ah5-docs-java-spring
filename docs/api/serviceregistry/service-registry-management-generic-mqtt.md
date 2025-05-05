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

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[Name](../primitives.md#name)>, which contains the names of the devices to delete.

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

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case the response payload is empty.

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

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is an optional [SystemQueryRequest](../data-models/system-query-request.md). The params can contain a [KeyValuePair](../primitives.md/#keyvaluepair) with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device information also returns (only if the provider supports it).

```
Topic: arrowhead/serviceregistry/management/system-query

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "params": {"verbose": <verbose-value>},
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
  "receiver": "sysop",
  "payload": {
    "entries": [
      {
        "name": "alert-consumer2",
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
  "receiver": "sysop",
  "payload": "Sort field is invalid. Only the following are allowed: [id, name, createdAt]"
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
        "name": "alert-consumer1",
        "metadata": {
        },
        "version": "1.1",
        "addresses": [
          "192.168.1.1"
        ],
        "deviceName": "alarm1"
      },
      {
        "name": "alert-consumer2",
        "metadata": {
        },
        "version": "1.1",
        "addresses": [
          "192.168.1.2"
        ],
        "deviceName": "alarm2"
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
  "receiver": "sysop",
  "payload": {
    "entries": [
      {
        "name": "alert-consumer1",
        "metadata": {},
        "version": "1.1.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.1"
          }
        ],
        "device": {
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
        "createdAt": "2025-05-04T20:08:37.278728600Z",
        "updatedAt": "2025-05-04T20:08:37.278728600Z"
      },
      {
        "name": "alert-consumer2",
        "metadata": {},
        "version": "1.1.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          }
        ],
        "device": {
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
  "receiver": "sysop",
  "payload": "Systems with names already exist: alert-consumer1, alert-consumer2"
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
        "name": "alert-consumer1",
        "metadata": {
        },
        "version": "1.2",
        "addresses": [
          "192.168.1.1"
        ],
        "deviceName": "alarm1"
      },
      {
        "name": "alert-consumer2",
        "metadata": {
        },
        "version": "1.2",
        "addresses": [
          "192.168.1.2"
        ],
        "deviceName": "alarm2"
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
  "receiver": "sysop",
  "payload": {
    "entries": [
      {
        "name": "alert-consumer1",
        "metadata": {},
        "version": "1.2.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.1"
          }
        ],
        "device": {
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
        "createdAt": "2025-05-04T20:08:37Z",
        "updatedAt": "2025-05-04T20:11:20.541557100Z"
      },
      {
        "name": "alert-consumer2",
        "metadata": {},
        "version": "1.2.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          }
        ],
        "device": {
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
  "receiver": "sysop",
  "payload": "System(s) not exists: alert-consumer1, alert-consumer2"
}
```

### system-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[Name](../primitives.md#name)>, which contains the names of the systems to delete.

```
arrowhead/serviceregistry/management/system-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": ["alert-consumer1", "alert-consumer2"]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case the response payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "sysop",
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
  "receiver": "sysop",
  "payload": {
    "entries": [
      {
        "name": "orchestration-lock-management",
        "createdAt": "2025-03-12T11:07:23Z",
        "updatedAt": "2025-03-12T11:07:23Z"
      },
      {
        "name": "orchestration-history-management",
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
  "receiver": "sysop",
  "payload": "Direction is invalid. Only ASC or DESC are allowed"
}
```

### service-definition-create 

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a [ServiceDefinitionListRequest](../data-models/service-definition-list-request.md).

```
{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": {
    "serviceDefinitionNames": [
      "alert-service1", "alert-service2"
    ]
  }
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. The response template payload is a [ServiceDefinitionListResponse](../data-models/service-definition-list-response.md).

```
{
  "status": 201,
  "traceId": "<trace-id>",
  "receiver": "sysop",
  "payload": {
    "entries": [
      {
        "name": "alert-service1",
        "createdAt": "2025-05-04T21:33:49.344720800Z",
        "updatedAt": "2025-05-04T21:33:49.344720800Z"
      },
      {
        "name": "alert-service2",
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
  "receiver": "sysop",
  "payload": "Service definition names already exists: alert-service1"
}
```

### service-definition-remove

The service operation **request** requires an [MQTTRequestTemplate](../data-models/mqtt-request-template.md) JSON encoded message in which the authentication is a proper [identity info](../../api/authentication_policy.md/#mqtt) and the payload is a List<[Name](../primitives.md#name)>, which contains the names of the service definitions to delete.

```
Topic: arrowhead/serviceregistry/management/service-definition-remove

{
  "traceId": "<trace-id>",
  "authentication": "<identity-info>",
  "responseTopic": "<response-topic>",
  "qosRequirement": <0|1|2>,
  "payload": ["alert-service1", "alert-service2"]
}
```

The service operation **responds** with an [MQTTResponseTemplate](../data-models/mqtt-response-template.md) JSON encoded message in which the status code is `200` if called successfully. In this case the response payload is empty.

```
{
  "status": 200,
  "traceId": "<trace-id>",
  "receiver": "sysop",
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

### service-query

### service-create

### service-update

### service-remove

### interface-template-query

### interface-template-create

### interface-template-remove