# service-registry-management GENERIC-HTTP

## Overview
This page describes the service-registry-management service, which enables systems (with operator role or proper permissions) to handle (register, update, revoke, lookup) [devices](#device-query), [systems](#system-query), [service instances](#service-query), [service definitions](#service-definition-query) and [interface template](#interface-template-query) in bulk. An example of this interaction is that an operator uses the Management Tool to register interface templates, systems, and service instances manually. The interfaces are implemented using protocol, encoding as stated in the following tables:

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

The service operation **request** requires an authorization bearer header and a [DeviceQueryRequest](../data-models/device-query-request.md) JSON encoded body.

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

The service operation **responds** with the status code `200` if called successfully. The response also contains a
[DeviceListResponse](../data-models/device-list-response.md) JSON encoded body.

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

The service operation **request** requires an authorization bearer header and a [DeviceListRequest](../data-models/device-list-request.md) JSON encoded body.

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

The service operation **responds** with the status code `201` if the device entities were successfully created. The response also contains a
[DeviceListResponse](../data-models/device-list-response.md) JSON encoded body.

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
The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
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

The service operation **request** requires an authorization bearer header and a [DeviceListRequest](../data-models/device-list-request.md) JSON encoded body.

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

The **error codes** are `400` if the request is malformed, `401` if the requester authentication was unsuccessful,
`403` if the authenticated requester has no permission and
`500` if an unexpected error happens. The error response also contains an
[ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Device(s) not exists: alarm001, alarm002",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "PUT /serviceregistry/mgmt/devices"
}
```

### device-remove

The service operation **request** requires an authorization bearer header and a List<[Name](../primitives.md#name)> as path parameter, which contains the names of the devices to delete.

```
DELETE /serviceregistry/mgmt/devices?names=alarm1&names=alarm2 HTTP/1.1
Authorization: Bearer <authorization-info>
```

The service operation **responds** with the status code `200` if called successfully. The success response does not contain any response body.

The **error codes** are, `400` if the request is malformed, `401` if the requester authentication was unsuccessful, `403` if the authenticated requester has no permission, `423` if entity is not removable and `500` if an unexpected error happens. The error response also contains an [ErrorResponse](../data-models/error-response.md) JSON encoded body.

```
{
  "errorMessage": "Device name list contains null or empty element",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "DELETE /serviceregistry/mgmt/devices"
}
```

### system-query

The service operation **request** requires an authorization bearer header. The URI contains a query parameter with the key "_verbose_" and a [Boolean](../primitives.md#boolean) value. If verbose is true, detailed device information also returns (only if the provider supports it). The request may optionally include a [SystemQueryRequest](../data-models/system-query-request.md) JSON encoded body.

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

The service operation **request** requires an authorization bearer header a [ SystemListRequest](../data-models/system-list-request.md) JSON encoded body.

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
The service operation **responds** with the status code `201` if the system entities were successfully created. The response also contains a
[SystemListResponse](../data-models/system-list-response.md) JSON encoded body.

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

The service operation **request** requires an authorization bearer header a [ SystemListRequest](../data-models/system-list-request.md) JSON encoded body.

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

The service operation **responds** with the status code `200` if the system entities were successfully updated. The response also contains a
[SystemListResponse](../data-models/system-list-response.md) JSON encoded body.

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

The service operation **request** requires an authorization bearer header and a List<[Name](../primitives.md#name)> as path parameter, which contains the names of the systems to delete.

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
### service-definition-create
### service-definition-remove
### service-query
### service-create
### service-update
### service-remove
### interface-template-query
### interface-template-create
### interface-template-remove

