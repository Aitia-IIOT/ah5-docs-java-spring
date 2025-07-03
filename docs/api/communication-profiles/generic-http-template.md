# Generic HTTP - Communication Profile

This page describes the Generic HTTP Communication Profile (CP), which templates an unsecure request-response based network communication using the [Hypertext Transfer Protocol](https://en.wikipedia.org/wiki/HTTP). 
## Characteristics

|Overview||
| --- | --- |
| ID | **generic_http** |
| Transfer protocol | [HTTP 1.1](https://datatracker.ietf.org/doc/html/rfc2616) |
| Data Encryption | N/A |
| Data Compression | N/A |
| Payload Format | [JSON](https://datatracker.ietf.org/doc/html/rfc8259) / Plain text |

## Service Interface Template

This section lists the interface properties that are necessarry to apply the Generic HTTP - CP as Service Interface and that Interface Design Descriptions (IDD) and/or the service registration must include. 

### Access Address List

List of unique identifiers assigned to the same device or node on a network where the given service is hosted from. An access address can be a Hostname, an IPv4 address, an IPv6 address or a MAC address. It is mandatory to publish them (at least one) in ServiceRegistry Core System.

### Access Port

A numerical identifier within a device or node on a network where the given service can be accessed. Valid port number range is 0-65535. It is mandatory to publish it in ServiceRegistry Core System.

### Base Path

The root [URL](https://datatracker.ietf.org/doc/html/rfc2616#section-3.2.2) segment that serves as a common prefix for a given service that goups a set of operations (endpoints). It is mandatory to specify it in IDDs and to publish in ServiceRegistry Core System.

### Operations

Set of endpoints, each of them is performing some kind of action related to the given service. An operation constists of a _method_ and a _path_. It is mandatory to specify them in IDDs and optional to publish in ServiceRegistry Core System.

#### Method

An [HTTP method](https://datatracker.ietf.org/doc/html/rfc2616#section-5.1.1), that applies to the given service-operation.

#### Path

The operation specific [URL](https://datatracker.ietf.org/doc/html/rfc2616#section-3.2.2) segment.

### Authorization Header

[Authorization request header](https://datatracker.ietf.org/doc/html/rfc2616#section-14.8) with [Bearer scheme](https://datatracker.ietf.org/doc/html/rfc6750#section-2.1) must be used for providing identity or access-right related data if required. It is mandatory to specify it in IDDs if authentication or access validation is required by a given service instance.

### Payload

The data to be transmitted to or from a given service-operation (endpoint). In case of hierarchical data, the payload must be in [JSON](https://datatracker.ietf.org/doc/html/rfc8259) data format and must be placed into the [HTTP Meassage Body](https://datatracker.ietf.org/doc/html/rfc2616#section-4.3). In case of non-hierarchical data, plain text data format can be used and placed into the [HTTP Meassage Body](https://datatracker.ietf.org/doc/html/rfc2616#section-4.3) or into the [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) as path variable or query parameter. It is mandatory to specify it in IDDs.

## Event Interface Template

This section lists the interface properties that are necessarry to apply the Generic HTTP - CP as Event Interface and that Interface Design Descriptions (IDD) and/or the event registration must include. 