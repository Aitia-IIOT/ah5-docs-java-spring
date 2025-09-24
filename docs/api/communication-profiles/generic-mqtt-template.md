# Generic MQTT - Communication Profile

This page describes the Generic MQTT Communication Profile (CP), which templates an unsecure request-response based network communication applied to the fundamentally publish-subscribe based [Message Queuing Telemetry Transport](https://en.wikipedia.org/wiki/MQTT) protocol. 

## Characteristics

|Overview||
| --- | --- |
| ID | **generic_mqtt** |
| Transfer protocol | [MQTT 3.1](https://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html) [MQTT 3.1.1](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html) |
| Data Encryption | N/A |
| Data Compression | N/A |

## Payload Aspects

Generic MQTT - CP regulates the message payload format and data model to be used, and it also regulates the service payload format (see the [message templates section](#message-templates)). However it does not regulate the service payload data model. All payload formats and data models are mandatory to specify in the Interface Design Descriptions (IDD).

## Service Interface Template

This section lists the interface properties that are necessarry to apply the Generic MQTT - CP as [Service](../../help/definitions.md#microservice-or-service) interface and that Interface Design Descriptions (IDD) and/or the service registration should include.


---

### Access Address List

List of unique identifiers assigned to the same [MQTT Broker](https://en.wikipedia.org/wiki/MQTT#MQTT_broker) on a network over which the given [Service](../../help/definitions.md#microservice-or-service) is accessable. An access address can be a hostname, an IPv4 address or an IPv6 address. It is mandatory to publish them (at least one) in the [Local Cloud](../../help/definitions.md#local-cloud).

---

### Access Port

A numerical identifier within a device or node on a network where the [MQTT Broker](https://en.wikipedia.org/wiki/MQTT#MQTT_broker) and therfore the given [Service](../../help/definitions.md#microservice-or-service) can be accessed. Valid port number range is 1-65535. It is mandatory to publish it in the [Local Cloud](../../help/definitions.md#local-cloud).

---

### Base Topic

The root topic segment that serves as a common prefix for a given [Service](../../help/definitions.md#microservice-or-service) that groups a set of [service-operations](../../help/definitions.md#service-operation). It is mandatory to specify it in IDDs and to publish in the [Local Cloud](../../help/definitions.md#local-cloud).

---

### Operations

Set of operation names. Together with the base topic prefix, each of them is a fully qualified MQTT Topic and is performing some kind of action related to the given [Service](../../help/definitions.md#microservice-or-service). It is mandatory to specify them in IDDs and is optional to publish in the [Local Cloud](../../help/definitions.md#local-cloud).

---

### Message Templates

#### Request Message Template

Messages with request intent always have to follow the [MQTTRequestTemplate](../data-models/mqtt-request-template.md) data model in [JSON](https://datatracker.ietf.org/doc/html/rfc8259) format.

#### Reponse Message Template

Messages with response intent always have to follow the [MQTTResponseTemplate](../data-models/mqtt-response-template.md) data model in [JSON](https://datatracker.ietf.org/doc/html/rfc8259) format.

#### Trace ID

A string reference choosen by the requester party to identifiy the request message (not mandatory). Service porviders are obliged to present the received trace id in the response messages.

#### Authentication

Authentication property must be used for providing identity or access related data if required in case of a given [Service](../../help/definitions.md#microservice-or-service). It is mandatory to specify it in IDDs if authentication or access validation is required by a given service instance.

##### Identity

If requester identity is required to be proven, then an [Identity Info](../../help/definitions.md#identity-info) or [Identiy Token](../../help/definitions.md#identity-token) should be provided in this property. The exact Identity data to be provided is depending on the applied [Authentication Policy](../authentication_policy.md).

##### Access

If the access right is required to be proven, then an [Access Token](../../help/definitions.md#access-token) should be provided in this property.

#### Response Topic

The topic on which the response is expected.

#### QoS Requirement

Required response [MQTT Quality of Service](https://www.hivemq.com/blog/mqtt-essentials-part-6-mqtt-quality-of-service-levels/).

#### Payload

The data to be transmitted to or from a given [service-operation](../../help/definitions.md#service-operation).

#### Request Parameters

The request parameters (alias params) are string key-value pairs to be transmitted to a given [service-operation](../../help/definitions.md#service-operation).

#### Response Status

Numerical values that indicate the result of a [service-operation](../../help/definitions.md#service-operation). It is mandatory to specify them in IDDs.

#### Receiver

Requester's identifier in the response message.

## Examples

### Request

```
Topic: <base-topic><delimiter><operation-name>
QoS: <0|1|2>

{
   "traceId": "<trace-id>",
   "authentication": "<access-token>",
   "responseTopic": "<response-topic>",
   "qosRequirement": <0|1|2>,
   "params": {
        "<key>": "<value>"
   },
   "payload": {
      <data>
   }
}
```

```
Topic: temperatureInfo/query-temperature
QoS: 2

{
   "traceId": "abc123",
   "authentication": "eyJhbGciOiJIUzI1NiIsI.eyJzdWIiOiIxMjM0NTY3ODkw.SflKxwRJSMeKKF2QT4fwpMeJ",
   "responseTopic": "example/response/topic",
   "qosRequirement": 2,
   "params": {
        "sensor-id": "sensor-abc",
        "scale": "celsius"
   },
   "payload": {
        "from": "2025-02-01T00:00:00Z",
        "to": "2025-02-06T23:59:00Z",
        "moreThan": 12,
        "lessThan": 24
    }
}
```

### Response

```
Topic: <response-topic>
QoS: <0|1|2>

{
   "status": <response-status>,
   "traceId": "<trace-id>",
   "receiver": "<receiver-id>",
   "payload": {
      <data>
   }
}
```

```
Topic: example/response/topic
QoS: 2

{
   "status": 200,
   "traceId": "abc123",
   "receiver": "ExampleConsumer",
   "payload": [
        {
            "timestamp": "2025-02-03T12:31:00Z",
            "celsius": 13,
        },
        {
            "timestamp": "2025-02-05T12:10:00Z",
            "celsius": 14
        }
    ]
}
```