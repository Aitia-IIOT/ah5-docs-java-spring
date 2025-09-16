# Generic MQTTS - Communication Profile

This page describes the Generic MQTTS Communication Profile (CP), which templates a secure request-response based network communication applied to the fundamentally publish-subscribe based [Message Queuing Telemetry Transport](https://en.wikipedia.org/wiki/MQTT) protocol. 

## Characteristics

|Overview||
| --- | --- |
| ID | **generic_mqtts** |
| Transfer protocol | [MQTT 3.1](https://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html) [MQTT 3.1.1](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html) |
| Data Encryption | [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) |
| Data Compression | N/A |

## Payload

Generic MQTTS - CP regulates the message payload format and data model to be used, and it also regulates the service payload format (see the [message templates section](./generic-mqtt-template.md#message-templates)). However it does not regulate the service payload data model. All payload formats and data models are mandatory to specify in the Interface Design Descriptions (IDD).

## Service Interface Template

At interface template level Generic MQTTS is identical with the [Generic MQTT - CP](./generic-mqtt-template.md#service-interface-template).