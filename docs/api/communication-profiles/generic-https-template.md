# Generic HTTPS - Communication Profile

This page describes the Generic HTTPS Communication Profile (CP), which templates a secure request-response based network communication using the [Hypertext Transfer Protocol](https://en.wikipedia.org/wiki/HTTP) over [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security). 

## Characteristics

|Overview||
| --- | --- |
| ID | **generic_https** |
| Transfer protocol | [HTTP 1.1](https://datatracker.ietf.org/doc/html/rfc2616) |
| Data Encryption | [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) |
| Data Compression | N/A |

## Payload Aspects

Generic HTTPS - CP does not regulate the service payload format. Any format (JSON, XML, Plain Text, etc.) may be used that best fits the requirements of the given [Service](../../help/definitions.md#microservice-or-service). The chosen data format and implemented data model are mandatory to specify in the Interface Design Descriptions (IDD).

## Service Interface Template

At interface template level Generic HTTPS is identical with the [Generic HTTP - CP](./generic-http-template.md#service-interface-template).