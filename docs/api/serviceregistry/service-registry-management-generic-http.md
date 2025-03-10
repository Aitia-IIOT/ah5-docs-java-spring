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
### system-create
### system-update
### system-remove
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

