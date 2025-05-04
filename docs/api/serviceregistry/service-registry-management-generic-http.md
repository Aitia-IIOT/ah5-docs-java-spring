# service-registry-management GENERIC-HTTP

## Overview
This page describes the service-registry-management service, which enables systems (with operator role or proper permissions) to handle (register, update, revoke, lookup) [devices](#device-query), [systems](#system-query), [service instances](#service-query), [service definitions](#service-definition-query) and [interface templates](#interface-template-query) in bulk. An example of this interaction is that an operator uses the Management Tool to register interface templates, systems, and service instances manually. The interfaces are implemented using protocol, encoding as stated in the following tables:

## Interface Description

**GENERIC-HTTP**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTP | 1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**GENERIC-HTTPS**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTPS | 1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [service-registry-management – Service Description](../../assets/sd/5_0_0/service-registry-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### device-query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and may optionally include a [DeviceQueryRequest](../data-models/device-query-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/devices/query HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```

The service operation **responds** with the status code `200` if called successfully. The response also contains a [DeviceListResponse](../data-models/device-list-response.md) JSON encoded body.

```
{
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
      "createdAt": "2025-03-10T09:09:58Z",
      "updatedAt": "2025-03-10T09:09:58Z"
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
      "createdAt": "2025-03-10T09:09:58Z",
      "updatedAt": "2025-03-10T09:09:58Z"
    }
  ],
  "count": 2
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Sort field is invalid. Only the following are allowed: [id, name, createdAt]",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/devices/query"
}
```

### device-create

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [DeviceListRequest](../data-models/device-list-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/devices HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```

The service operation **responds** with the status code `201` if the device entities were successfully created. The response also contains a [DeviceListResponse](../data-models/device-list-response.md) JSON encoded body.

```
{
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
      "createdAt": "2025-03-10T09:09:57.989904100Z",
      "updatedAt": "2025-03-10T09:09:57.989904100Z"
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
      "createdAt": "2025-03-10T09:09:58.010712400Z",
      "updatedAt": "2025-03-10T09:09:58.010712400Z"
    }
  ],
  "count": 2
}
```
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Device with names already exists: alarm1, alarm2",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/devices"
}
```

### device-update

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [DeviceListRequest](../data-models/device-list-request.md) JSON encoded body.

```
PUT /serviceregistry/mgmt/devices HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```

The service operation **responds** with the status code `200` if called successfully. The response also contains a [DeviceListResponse](../data-models/device-list-response.md) JSON encoded body.

```
{
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
      "createdAt": "2025-03-10T09:09:58Z",
      "updatedAt": "2025-03-10T09:09:58Z"
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
      "createdAt": "2025-03-10T09:09:58Z",
      "updatedAt": "2025-03-10T09:09:58Z"
    }
  ],
  "count": 2
}
```

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Device(s) not exists: alarm001, alarm002",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "PUT /serviceregistry/mgmt/devices"
}
```

### device-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[Name](../primitives.md#name)> as path parameter, which contains the names of the devices to delete.

```
DELETE /serviceregistry/mgmt/devices?names=alarm1&names=alarm2 HTTP/1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `423` if entity is not removable and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Device name list contains null or empty element",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "DELETE /serviceregistry/mgmt/devices"
}
```

### system-query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http). The URI contains a query parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device information also returns (only if the provider supports it). The request may optionally include a [SystemQueryRequest](../data-models/system-query-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/systems/query?verbose=<verbose-value> HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```

The service operation **responds** with the status code `200` if called successfully and with a [SystemListResponse](../data-models/system-list-response.md) JSON encoded body.

```
{
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
      "device": {
        "name": "alarm2"
      },
      "createdAt": "2025-03-14T13:08:22Z",
      "updatedAt": "2025-03-14T13:08:22Z"
    }
  ],
  "count": 2
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "The page size cannot be larger than 1000",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/systems/query"
}
```

### system-create

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ SystemListRequest](../data-models/system-list-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/systems HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```
The service operation **responds** with the status code `201` if the system entities were successfully created. The response also contains a [SystemListResponse](../data-models/system-list-response.md) JSON encoded body.

```
{
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
            "address": "3a:f7:9c:12:8e:b5"
          }
        ],
        "createdAt": "2025-03-14T13:07:34Z",
        "updatedAt": "2025-03-14T13:07:34Z"
      },
      "createdAt": "2025-03-14T13:08:21.856389Z",
      "updatedAt": "2025-03-14T13:08:21.856389Z"
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
            "address": "3a:f7:9c:12:8e:bb"
          }
        ],
        "createdAt": "2025-03-14T13:07:34Z",
        "updatedAt": "2025-03-14T13:07:34Z"
      },
      "createdAt": "2025-03-14T13:08:21.858647200Z",
      "updatedAt": "2025-03-14T13:08:21.858647200Z"
    }
  ],
  "count": 2
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission  and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Systems with names already exist: alert-consumer1, alert-consumer2",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/systems"
}
```

### system-update

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ SystemListRequest](../data-models/system-list-request.md) JSON encoded body.

```
PUT /serviceregistry/mgmt/systems HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```

The service operation **responds** with the status code `200` if the system entities were successfully updated. The response also contains a [SystemListResponse](../data-models/system-list-response.md) JSON encoded body.

```
{
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
            "address": "3a:f7:9c:12:8e:b5"
          }
        ],
        "createdAt": "2025-03-14T13:07:34Z",
        "updatedAt": "2025-03-14T13:07:34Z"
      },
      "createdAt": "2025-03-14T13:08:22Z",
      "updatedAt": "2025-03-14T13:51:03.159696600Z"
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
            "address": "3a:f7:9c:12:8e:bb"
          }
        ],
        "createdAt": "2025-03-14T13:07:34Z",
        "updatedAt": "2025-03-14T13:07:34Z"
      },
      "createdAt": "2025-03-14T13:08:22Z",
      "updatedAt": "2025-03-14T13:51:03.169626100Z"
    }
  ],
  "count": 2
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission  and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Duplicated system name: alert-consumer1",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "PUT /serviceregistry/mgmt/systems"
}
```

### system-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[Name](../primitives.md#name)> as path parameter, which contains the names of the systems to delete.

```
DELETE /serviceregistry/mgmt/systems?names=alert-consumer1&names=alert-consumer2 HTTP/1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Invalid authorization header",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```
### service-definition-query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and may optionally include a [PageRequest](../data-models/page-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/service-definitions/query HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "page": 2,
  "size": 4,
  "direction": "DESC",
  "sortField": "name"
}
```

The service operation **responds** with the status code `200` if called successfully. The response also contains a [ServiceDefinitionListResponse](../data-models/service-definition-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "name": "orchestration",
      "createdAt": "2025-03-12T11:07:23Z",
      "updatedAt": "2025-03-12T11:07:23Z"
    },
    {
      "name": "monitor",
      "createdAt": "2025-01-31T09:14:54Z",
      "updatedAt": "2025-01-31T09:14:54Z"
    },
    {
      "name": "kelvin-info2",
      "createdAt": "2025-03-15T19:47:47Z",
      "updatedAt": "2025-03-15T19:47:47Z"
    },
    {
      "name": "kelvin-info1",
      "createdAt": "2025-03-15T19:47:47Z",
      "updatedAt": "2025-03-15T19:47:47Z"
    }
  ],
  "count": 24
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Requester has no management permission",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN",
  "origin": "http://localhost:8443/serviceregistry/mgmt/service-definitions/query"
}
```

### service-definition-create

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceDefinitionListRequest](../data-models/service-definition-list-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/service-definitions HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "serviceDefinitionNames": [
    "alert-service1", "alert-service2"
  ]
}
```

The service operation **responds** with the status code `201` if the service definition entities were successfully created. The response also contains a [ServiceDefinitionListResponse](../data-models/service-definition-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "name": "alert-service1",
      "createdAt": "2025-03-15T19:31:03.728040300Z",
      "updatedAt": "2025-03-15T19:31:03.728040300Z"
    },
    {
      "name": "alert-service2",
      "createdAt": "2025-03-15T19:31:03.732592400Z",
      "updatedAt": "2025-03-15T19:31:03.732592400Z"
    }
  ],
  "count": 2
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "The specified name does not match the naming convention: alert@service1",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/service-definitions"
}
```

### service-definition-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[Name](../primitives.md#name)> as path parameter, which contains the names of the service definitions to delete.

```
DELETE /serviceregistry/mgmt/service-definitions?names=alert-service1&names=alert-service2 HTTP/1.1
Authorization: Bearer <authorization-info>
```
The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Service definition name list is missing or empty",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "DELETE /serviceregistry/mgmt/service-definitions"
}
```

### service-query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceQueryRequest](../data-models/service-query-request.md) JSON encoded body. The URI contains a query parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed system and device information also returns (only if the provider supports it).

```
POST /serviceregistry/mgmt/service-instances/query?verbose=<verbose-value> HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
    "alert-service1"
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
    "generic-mqtt"
  ],
  "interfacePropertyRequirementsList": [
    {
      "operations": { "op": "CONTAINS", "value": "warn"}
    }
  ],
  "policies": [
    "NONE"
  ]
}
```

The service operation **responds** with the status code `200` if called successfully. The response also contains a [ServiceListResponse](../data-models/service-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "instanceId": "alert-consumer1::alert-service1::1.0.0",
      "provider": {
        "name": "alert-consumer1",
        "metadata": {},
        "version": "1.1.0",
        "createdAt": "2025-03-15T20:22:44Z",
        "updatedAt": "2025-03-15T20:22:44Z"
      },
      "serviceDefinition": {
        "name": "alert-service1",
        "createdAt": "2025-03-15T20:21:43Z",
        "updatedAt": "2025-03-15T20:21:43Z"
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
          "templateName": "generic-mqtt",
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
      "createdAt": "2025-03-15T21:52:40Z",
      "updatedAt": "2025-03-15T21:52:40Z"
    },
    {
      "instanceId": "alert-consumer2::alert-service1::1.0.0",
      "provider": {
        "name": "alert-consumer2",
        "metadata": {},
        "version": "1.1.0",
        "createdAt": "2025-03-15T20:22:44Z",
        "updatedAt": "2025-03-15T20:22:44Z"
      },
      "serviceDefinition": {
        "name": "alert-service1",
        "createdAt": "2025-03-15T20:21:43Z",
        "updatedAt": "2025-03-15T20:21:43Z"
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
          "templateName": "generic-mqtt",
          "protocol": "tcp",
          "policy": "NONE",
          "properties": {
            "accessAddresses": [
              "192.168.1.3"
            ],
            "accessPort": 1883,
            "baseTopic": "heat-alert",
            "operations": [
              "warn"
            ]
          }
        }
      ],
      "createdAt": "2025-03-15T21:52:40Z",
      "updatedAt": "2025-03-15T21:52:40Z"
    }
  ],
  "count": 2
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Sort field is invalid. Only the following are allowed: [id, name, createdAt]",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/service-instances/query"
}
```

### service-create

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceCreateListRequest](../data-models/service-create-list-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/service-instances HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "instances": [
    {
      "systemName": "alert-consumer1",
      "serviceDefinitionName": "alert-service1",
      "version": "",
      "expiresAt": "2028-01-01T00:00:00Z",
      "metadata": {
        "delay": {"value": 200, "unit": "ms"}
      },
      "interfaces": [
        {
          "templateName": "generic-mqtt",
          "protocol": "tcp", 
          "policy": "NONE",
          "properties": {
            "accessAddresses": ["192.168.1.3"],
            "accessPort": 1883,
            "baseTopic": "heat-alert", "operations": ["alert", "warn"]
          }
        }
      ]
    },
    {
      "systemName": "alert-consumer2",
      "serviceDefinitionName": "alert-service1",
      "version": "",
      "expiresAt": "2027-01-01T00:00:00Z",
      "metadata": {
        "delay": {"value": 200, "unit": "ms"}
      },
      "interfaces": [
        {
          "templateName": "generic-mqtt",
          "protocol": "tcp", 
          "policy": "NONE",
          "properties": {
            "accessAddresses": ["192.168.1.3"],
            "accessPort": 1883,
            "baseTopic": "heat-alert", "operations": ["warn"]
          }
        }
      ]
    }
  ]
}
```
The service operation **responds** with the status code `201` if the service instance entities were created successfully. The response also contains a [ServiceListResponse](../data-models/service-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "instanceId": "alert-consumer1::alert-service1::1.0.0",
      "provider": {
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
              "address": "3a:f7:9c:12:8e:b5"
            }
          ],
          "createdAt": "2025-03-14T13:07:34Z",
          "updatedAt": "2025-03-14T13:07:34Z"
        },
        "createdAt": "2025-03-15T20:22:44Z",
        "updatedAt": "2025-03-15T20:22:44Z"
      },
      "serviceDefinition": {
        "name": "alert-service1",
        "createdAt": "2025-03-15T20:21:43Z",
        "updatedAt": "2025-03-15T20:21:43Z"
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
          "templateName": "generic-mqtt",
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
      "createdAt": "2025-03-15T21:52:40.389503Z",
      "updatedAt": "2025-03-15T21:52:40.389503Z"
    },
    {
      "instanceId": "alert-consumer2::alert-service1::1.0.0",
      "provider": {
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
              "address": "3a:f7:9c:12:8e:bb"
            }
          ],
          "createdAt": "2025-03-14T13:07:34Z",
          "updatedAt": "2025-03-14T13:07:34Z"
        },
        "createdAt": "2025-03-15T20:22:44Z",
        "updatedAt": "2025-03-15T20:22:44Z"
      },
      "serviceDefinition": {
        "name": "alert-service1",
        "createdAt": "2025-03-15T20:21:43Z",
        "updatedAt": "2025-03-15T20:21:43Z"
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
          "templateName": "generic-mqtt",
          "protocol": "tcp",
          "policy": "NONE",
          "properties": {
            "accessAddresses": [
              "192.168.1.3"
            ],
            "accessPort": 1883,
            "baseTopic": "heat-alert",
            "operations": [
              "warn"
            ]
          }
        }
      ],
      "createdAt": "2025-03-15T21:52:40.394058800Z",
      "updatedAt": "2025-03-15T21:52:40.394058800Z"
    }
  ],
  "count": 2
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "type": "about:blank",
  "title": "Bad Request",
  "status": 400,
  "detail": "Failed to read request",
  "instance": "/serviceregistry/mgmt/service-instances"
}
```

### service-update

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceUpdateListRequest](../data-models/service-update-list-request.md) JSON encoded body.

```
PUT /serviceregistry/mgmt/service-instances HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "instances": [
    {
      "instanceId": "alert-consumer1::alert-service1::1.0.0",
      "expiresAt": "2028-01-01T00:00:00Z",
      "metadata": {
        "delay": {"value": 200, "unit": "ms"}
      },
      "interfaces": [
        {
          "templateName": "generic-mqtt",
          "protocol": "tcp", "policy": "NONE",
          "properties": {
            "accessAddresses": ["192.168.1.3"],
            "accessPort": 1883,
            "baseTopic": "heat-alert", "operations": ["alert", "warn", "info"]
          }
        }
      ]
    },
    {
      "instanceId": "alert-consumer2::alert-service1::1.0.0",
      "expiresAt": "2027-01-01T00:00:00Z",
      "metadata": {
        "delay": {"value": 200, "unit": "ms"}
      },
      "interfaces": [
        {
          "templateName": "generic-mqtt",
          "protocol": "tcp", "policy": "NONE",
          "properties": {
            "accessAddresses": ["192.168.1.3"],
            "accessPort": 1883,
            "baseTopic": "heat-alert", "operations": ["warn"]
          }
        }
      ]
    }
  ]
}
```

The service operation **responds** with the status code `200` if the service instance entities were updated successfully. The response also contains a [ServiceListResponse](../data-models/service-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "instanceId": "alert-consumer1::alert-service1::1.0.0",
      "provider": {
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
              "address": "3a:f7:9c:12:8e:b5"
            }
          ],
          "createdAt": "2025-03-14T13:07:34Z",
          "updatedAt": "2025-03-14T13:07:34Z"
        },
        "createdAt": "2025-03-15T20:22:44Z",
        "updatedAt": "2025-03-15T20:22:44Z"
      },
      "serviceDefinition": {
        "name": "alert-service1",
        "createdAt": "2025-03-15T20:21:43Z",
        "updatedAt": "2025-03-15T20:21:43Z"
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
          "templateName": "generic-mqtt",
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
      "createdAt": "2025-03-15T21:52:40Z",
      "updatedAt": "2025-03-15T22:30:44.209238800Z"
    },
    {
      "instanceId": "alert-consumer2::alert-service1::1.0.0",
      "provider": {
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
              "address": "3a:f7:9c:12:8e:bb"
            }
          ],
          "createdAt": "2025-03-14T13:07:34Z",
          "updatedAt": "2025-03-14T13:07:34Z"
        },
        "createdAt": "2025-03-15T20:22:44Z",
        "updatedAt": "2025-03-15T20:22:44Z"
      },
      "serviceDefinition": {
        "name": "alert-service1",
        "createdAt": "2025-03-15T20:21:43Z",
        "updatedAt": "2025-03-15T20:21:43Z"
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
          "templateName": "generic-mqtt",
          "protocol": "tcp",
          "policy": "NONE",
          "properties": {
            "accessAddresses": [
              "192.168.1.3"
            ],
            "accessPort": 1883,
            "baseTopic": "heat-alert",
            "operations": [
              "warn"
            ]
          }
        }
      ],
      "createdAt": "2025-03-15T21:52:40Z",
      "updatedAt": "2025-03-15T22:30:44.270475Z"
    }
  ],
  "count": 2
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Instance id is empty",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER"
}
```

### service-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[Name](../primitives.md#name)> as path parameter, which contains the identitifers of the service instances that need to be removed.

```
DELETE /serviceregistry/mgmt/service-instances?serviceInstances=alert-consumer1%3A%3Aalert-service1%3A%3A1.0.0&serviceInstances=alert-consumer2%3A%3Aalert-service1%3A%3A1.0.0 HTTP/1.1
Authorization: Bearer <authorization-info>
```
The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Requester has no management permission",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN",
  "origin": "http://localhost:8443/serviceregistry/mgmt/service-instances"
}
```

### interface-template-query

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and may optionally include an [InterfaceTemplateQueryRequest](../data-models/interface-template-query-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/interface-templates/query HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```

The service operation **responds** with the status code `200` called successfully. The response also contains an [InterfaceTemplateListResponse](../data-models/interface-template-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "name": "generic-mqtt",
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
            "NAME"
          ]
        }
      ],
      "createdAt": "2024-12-09T18:52:48Z",
      "updatedAt": "2024-12-09T18:52:48Z"
    }
  ],
  "count": 3
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "type": "about:blank",
  "title": "Bad Request",
  "status": 400,
  "detail": "Failed to read request",
  "instance": "/serviceregistry/mgmt/interface-templates/query"
}
```

### interface-template-create

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [InterfaceTemplateListRequest](../data-models/interface-template-list-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/interface-templates HTTP/1.1
Authorization: Bearer <authorization-info>

{
  "interfaceTemplates": [
    {
      "name": "custom-ftp",
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
      "name": "my-awesome-ftp",
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
```
The service operation **responds** with the status code `201` if the interface template entities were successfully created. The response also contains an [InterfaceTemplateListResponse](../data-models/interface-template-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "name": "my-awesome-ftp",
      "protocol": "tcp",
      "propertyRequirements": [
        {
          "name": "accessAddresses",
          "mandatory": true,
          "validator": "NOT_EMPTY_ADDRESS_LIST",
          "validatorParams": []
        }
      ],
      "createdAt": "2025-03-15T23:09:00.882593100Z",
      "updatedAt": "2025-03-15T23:09:00.882593100Z"
    },
    {
      "name": "custom-ftp",
      "protocol": "tcp",
      "propertyRequirements": [
        {
          "name": "accessAddresses",
          "mandatory": true,
          "validator": "NOT_EMPTY_ADDRESS_LIST",
          "validatorParams": []
        }
      ],
      "createdAt": "2025-03-15T23:09:00.892877600Z",
      "updatedAt": "2025-03-15T23:09:00.892877600Z"
    }
  ],
  "count": 2
}
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Interface template already exists: custom-ftp",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/interface-templates"
}
```

### interface-template-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[InterfaceTemplate](../primitives.md#interfacetemplate)> as path parameter, which contains the string identifier of the interface descriptors that need to be removed.

```
DELETE /serviceregistry/mgmt/interface-templates?names=custom-ftp&names=my-awesome-ftp HTTP/1.1
Authorization: Bearer <authorization-info>
```
The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "No authorization header has been provided",
  "errorCode": 401,
  "exceptionType": "AUTH"
}
```