# serviceRegistryManagement IDD

**generic_http & generic_https**

## Overview
This page describes the serviceRegistryManagement service, which enables systems (with operator role or proper permissions) to handle (register, update, revoke, lookup) [devices](#device-query), [systems](#system-query), [service instances](#service-query), [service definitions](#service-definition-query) and [interface templates](#interface-template-query) in bulk. An example of this interaction is that an operator uses the Management Tool to register interface templates, systems, and service instances manually. The interfaces are implemented using protocol, encoding as stated in the following tables:

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

Hereby the **Interface Design Description** (IDD) is provided to the [serviceRegistryManagement – Service Description](../../assets/sd/5_0_0/service-registry-management_sd.pdf). For further details about how this service is meant to be used, please consult that document.

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
      "createdAt": "2025-03-10T09:09:58Z",
      "updatedAt": "2025-03-10T09:09:58Z"
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
      "name": "ALARM1",
      "metadata": {
        "volume": { "value": 100, "unit": "dB"}
      },
      "addresses": [
        "3a:f7:9c:12:8e:b5"
      ]
    },
    {
      "name": "ALARM2",
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
      "createdAt": "2025-03-10T09:09:57.989904100Z",
      "updatedAt": "2025-03-10T09:09:57.989904100Z"
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
  "errorMessage": "Device with names already exists: ALARM1, ALARM2",
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
      "name": "ALARM1",
      "metadata": {
        "volume": { "value": 100, "unit": "dB"}
      },
      "addresses": [
        "4a:f7:9c:12:8e:b5"
      ]
    },
    {
      "name": "ALARM2",
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
      "createdAt": "2025-03-10T09:09:58Z",
      "updatedAt": "2025-03-10T09:09:58Z"
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
  "errorMessage": "Device(s) not exists: ALARM001, ALARM002",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "PUT /serviceregistry/mgmt/devices"
}
```

### device-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[DeviceName](../primitives.md#devicename)> as path parameter, which contains the names of the devices to delete.

```
DELETE /serviceregistry/mgmt/devices?names=ALARM1&names=ALARM2 HTTP/1.1
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

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http). The URI can contain an optional query parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device information also returns (only if the provider supports it). The request may optionally include a [SystemQueryRequest](../data-models/system-query-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/systems/query?verbose=false HTTP/1.1
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
        "name": "ALARM2"
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

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [SystemListRequest](../data-models/system-list-request.md) JSON encoded body.

```
POST /serviceregistry/mgmt/systems HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```
The service operation **responds** with the status code `201` if the system entities were successfully created. The response also contains a [SystemListResponse](../data-models/system-list-response.md) JSON encoded body.

```
{
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
  "errorMessage": "Systems with names already exist: AlertConsumer1, AlertConsumer2",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/systems"
}
```

### system-update

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [SystemListRequest](../data-models/system-list-request.md) JSON encoded body.

```
PUT /serviceregistry/mgmt/systems HTTP/1.1
Authorization: Bearer <authorization-info>

{
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
```

The service operation **responds** with the status code `200` if the system entities were successfully updated. The response also contains a [SystemListResponse](../data-models/system-list-response.md) JSON encoded body.

```
{
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
  "errorMessage": "Duplicated system name: AlertConsumer1",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "PUT /serviceregistry/mgmt/systems"
}
```

### system-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[SystemName](../primitives.md#systemname)> as path parameter, which contains the names of the systems to delete.

```
DELETE /serviceregistry/mgmt/systems?names=AlertConsumer1&names=AlertConsumer2 HTTP/1.1
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
      "name": "kelvinInfo2",
      "createdAt": "2025-03-15T19:47:47Z",
      "updatedAt": "2025-03-15T19:47:47Z"
    },
    {
      "name": "kelvinInfo1",
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
    "alertService1", "alertService2"
  ]
}
```

The service operation **responds** with the status code `201` if the service definition entities were successfully created. The response also contains a [ServiceDefinitionListResponse](../data-models/service-definition-list-response.md) JSON encoded body.

```
{
  "entries": [
    {
      "name": "alertService1",
      "createdAt": "2025-03-15T19:31:03.728040300Z",
      "updatedAt": "2025-03-15T19:31:03.728040300Z"
    },
    {
      "name": "alertService2",
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

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[ServiceName](../primitives.md#servicename)> as path parameter, which contains the names of the service definitions to delete.

```
DELETE /serviceregistry/mgmt/service-definitions?names=alertService1&names=alertService2 HTTP/1.1
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

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a [ServiceQueryRequest](../data-models/service-query-request.md) JSON encoded body. The URI can contain an optional query parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed system and device information also returns (only if the provider supports it).

```
POST /serviceregistry/mgmt/service-instances/query?verbose=false HTTP/1.1
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
      "instanceId": "AlertProvider1|alertService1|1.0.0",
      "provider": {
        "name": "AlertProvider1",
        "metadata": {},
        "version": "1.1.0",
        "createdAt": "2025-03-15T20:22:44Z",
        "updatedAt": "2025-03-15T20:22:44Z"
      },
      "serviceDefinition": {
        "name": "alertService1",
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
      "createdAt": "2025-03-15T21:52:40Z",
      "updatedAt": "2025-03-15T21:52:40Z"
    }    
  ],
  "count": 1
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
      "systemName": "AlertProvider1",
      "serviceDefinitionName": "alertService1",
      "version": "",
      "expiresAt": "2028-01-01T00:00:00Z",
      "metadata": {
        "delay": {"value": 200, "unit": "ms"}
      },
      "interfaces": [
        {
          "templateName": "generic_mqtt",
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
      "systemName": "AlertProvider2",
      "serviceDefinitionName": "alertService2",
      "version": "",
      "expiresAt": "2027-01-01T00:00:00Z",
      "metadata": {
        "delay": {"value": 200, "unit": "ms"}
      },
      "interfaces": [
        {
          "templateName": "generic_mqtt",
          "protocol": "tcp", 
          "policy": "NONE",
          "properties": {
            "accessAddresses": ["192.168.1.4"],
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
        "name": "alertService1",
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
      "createdAt": "2025-03-15T21:52:40.389503Z",
      "updatedAt": "2025-03-15T21:52:40.389503Z"
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
        "name": "alertService2",
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
  "errorMessage": "Service definition name is empty",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/service-instances"
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
      "instanceId": "AlertProvider1|alertService1|1.0.0",
      "expiresAt": "2028-01-01T00:00:00Z",
      "metadata": {
        "delay": {"value": 200, "unit": "ms"}
      },
      "interfaces": [
        {
          "templateName": "generic_mqtt",
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
      "instanceId": "AlertProvider2|alertService2|1.0.0",
      "expiresAt": "2027-01-01T00:00:00Z",
      "metadata": {
        "delay": {"value": 200, "unit": "ms"}
      },
      "interfaces": [
        {
          "templateName": "generic_mqtt",
          "protocol": "tcp", "policy": "NONE",
          "properties": {
            "accessAddresses": ["192.168.1.4"],
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
        "name": "alertService1",
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
      "createdAt": "2025-03-15T21:52:40Z",
      "updatedAt": "2025-03-15T22:30:44.209238800Z"
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
        "name": "alertService2",
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
  "exceptionType": "INVALID_PARAMETER",
  "origin": "PUT /serviceregistry/mgmt/service-instances"
}
```

### service-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[ServiceInstanceID](../primitives.md#serviceinstanceid)> as path parameter, which contains the identitifers of the service instances that need to be removed.

```
DELETE /serviceregistry/mgmt/service-instances?serviceInstances=AlertProvider1%7CalertService1%7C1.0.0&serviceInstances=AlertProvider2%7CalertService2%7C1.0.0 HTTP/1.1
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
```

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
"The specified interface template name does not match the naming convention: "
{
  "errorMessage": "The specified interface template name does not match the naming convention: general@mqtt",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/interface-templates/query"
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
```
The service operation **responds** with the status code `201` if the interface template entities were successfully created. The response also contains an [InterfaceTemplateListResponse](../data-models/interface-template-list-response.md) JSON encoded body.

```
{
  "entries": [
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
      "createdAt": "2025-03-15T23:09:00.882593100Z",
      "updatedAt": "2025-03-15T23:09:00.882593100Z"
    },
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
  "errorMessage": "Interface template already exists: custom_ftp",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /serviceregistry/mgmt/interface-templates"
}
```

### interface-template-remove

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and a List<[InterfaceName](../primitives.md#interfacename)> as path parameter, which contains the string identifier of the interface templates that need to be removed.

```
DELETE /serviceregistry/mgmt/interface-templates?names=custom_ftp&names=my_awesome_ftp HTTP/1.1
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