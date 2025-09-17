# Generic HTTP - Communication Profile

This page describes the Generic HTTP Communication Profile (CP), which templates an unsecure request-response based network communication using the [Hypertext Transfer Protocol](https://en.wikipedia.org/wiki/HTTP). 

## Characteristics

|Overview||
| --- | --- |
| ID | **generic_http** |
| Transfer protocol | [HTTP 1.1](https://datatracker.ietf.org/doc/html/rfc2616) |
| Data Encryption | N/A |
| Data Compression | N/A |

## Payload Aspects

Generic HTTP - CP does not regulate the service payload format. Any format (JSON, XML, Plain Text, etc.) may be used that best fits the requirements of the given [Service](../../help/definitions.md#microservice-or-service). The chosen data format and implemented data model are mandatory to specify in the Interface Design Descriptions (IDD).

## Service Interface Template

This section lists the interface properties that are necessarry to apply the Generic HTTP - CP as [Service](../../help/definitions.md#microservice-or-service) interface and that Interface Design Descriptions (IDD) and/or the service registration should include. 

---

### Access Address List

List of unique identifiers assigned to the same device or node on a network where the given [Service](../../help/definitions.md#microservice-or-service) is hosted from. An access address can be a hostname, an IPv4 address or an IPv6 address. It is mandatory to publish them (at least one) in the [Local Cloud](../../help/definitions.md#local-cloud).

---

### Access Port

A numerical identifier within a device or node on a network where the given [Service](../../help/definitions.md#microservice-or-service) can be accessed. Valid port number range is 1-65535. It is mandatory to publish it in the [Local Cloud](../../help/definitions.md#local-cloud).

---

### Base Path

The root [URL](https://datatracker.ietf.org/doc/html/rfc2616#section-3.2.2) segment that serves as a common prefix for a given [Service](../../help/definitions.md#microservice-or-service) that goups a set of [service-operations](../../help/definitions.md#service-operation) (endpoints). It is mandatory to specify it in IDDs and to publish in the [Local Cloud](../../help/definitions.md#local-cloud).

---

### Operations

Set of endpoints, each of them is performing some kind of action related to the given [Service](../../help/definitions.md#microservice-or-service). An operation constists of a _method_ and a _path_. It is mandatory to specify them in IDDs and is optional to publish in the [Local Cloud](../../help/definitions.md#local-cloud).

#### Method

An [HTTP method](https://datatracker.ietf.org/doc/html/rfc2616#section-5.1.1), that applies to the given [service-operation](../../help/definitions.md#service-operation).

#### Path

The [service-operation](../../help/definitions.md#service-operation) specific [URL](https://datatracker.ietf.org/doc/html/rfc2616#section-3.2.2) segment.

---

### Authorization Header

[HTTP Authorization request header](https://datatracker.ietf.org/doc/html/rfc2616#section-14.8) with [Bearer scheme](https://datatracker.ietf.org/doc/html/rfc6750#section-2.1) must be used for providing identity or access related data if required in case of a given [Service](../../help/definitions.md#microservice-or-service). It is mandatory to specify it in IDDs if authentication or access validation is required by a given service instance.

#### Identity

If requester identity is required to be proven, then an [Identity Info](../../help/definitions.md#identity-info) or [Identiy Token](../../help/definitions.md#identity-token) should be provided in this header. The exact Identity data to be provided is depending on the applied [Authentication Policy](../authentication_policy.md).

#### Access

If the access right is required to be proven, then an [Access Token](../../help/definitions.md#access-token) should be provided in this header.

---

### Payload

The data to be transmitted to or from a given [service-operation](../../help/definitions.md#service-operation) (endpoint). In case of hierarchical data, the payload must be in [JSON](https://datatracker.ietf.org/doc/html/rfc8259) data format and must be placed into the [HTTP Meassage Body](https://datatracker.ietf.org/doc/html/rfc2616#section-4.3). In case of non-hierarchical data, text data format can be used and placed into the [HTTP Meassage Body](https://datatracker.ietf.org/doc/html/rfc2616#section-4.3) or into the request [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) as URI encoded path variable or query parameter. It is mandatory to specify it in IDDs.

---

### Response Status

The [HTTP Response Status](https://datatracker.ietf.org/doc/html/rfc2616#section-6.1.1) that indicates the result of a [service-operation](../../help/definitions.md#service-operation). It is mandatory to specify it in IDDs.

## Examples

### Request

```
<METHOD> /<basepath>/<operation-path>/<path-variable>?<query-parameter-key>=<query-parameter-value> HTTP/1.1
Host: <access-address>:<access-port>
Accept: application/json
Authorization: Bearer <access-token>
Content-Type: application/json

<body>
```

---

```
POST /temperatureInfo/query-temperature/sensor-abc?scale=celsius HTTP/1.1
Host: 192.168.0.103:4132
Accept: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsI.eyJzdWIiOiIxMjM0NTY3ODkw.SflKxwRJSMeKKF2QT4fwpMeJ
Content-Type: application/json

{
    "from": "2025-02-01T00:00:00Z",
    "to": "2025-02-06T23:59:00Z",
    "moreThan": 12,
    "lessThan": 24
}
```

### Response

```
HTTP/1.1 <STATUS>
Date: <response-date>
Content-Type: application/json; charset=UTF-8

<body>
```

---

```
HTTP/1.1 200 OK
Date: Wed, 07 Feb 2025 12:00:00 GMT
Content-Type: application/json; charset=UTF-8

[
    {
        "timestamp": "2025-02-03T12:31:00Z",
        "celsius": 13,
    },
    {
        "timestamp": "2025-02-05T12:10:00Z",
        "celsius": 14
    }
]
```