# Primitives

## Address

A string representation of a network address. An address can be a version 4 IP address, a version 6 IP address,
DNS name or MAC address.

## AddressType

String value of a network address type. Could be only `HOSTNAME`, `IPV4`, `IPV6` or `MAC`.

## AuthenticationMethod

A string representation of an authentication method. Currently, only `PASSWORD` is supported.`

## Boolean

One out of `true` or `false`.

## DateTime

Pinpoints a moment in time in the format of ISO8601 standard `yyyy-mm-ddThh:MM:ssZ`, where ”yyyy” denotes
year (4 digits), ”mm” denotes month starting from 01, ”dd” denotes day starting from 01, ”T” is the separator
between date and time part, ”hh” denotes hour in the 24-hour format (00-23), ”MM” denotes minute (00-59),
”ss” denotes second (00-59). ”Z” indicates that the time is in UTC. An example of a valid date/time string is
”2024-12-05T12:00:00Z”

## Direction
The direction of a sorting operation. Possible values are the String representation of ascending (`ASC`) or descending (`DESC`) order.

## ErrorType

String value of the error type. Could be `ARROWHEAD`, `INVALID_PARAMETER`, `AUTH`, `FORBIDDEN`, `DATA_NOT_FOUND`,
`TIMEOUT`, `LOCKED`, `INTERNAL_SERVER_ERROR` or `EXTERNAL_SERVER_ERROR`.

## LogSeverity

Alias for a String value that describes the kind and seriousness of a log message. Could be `ALL`, `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR` or `FATAL`.

## MQTTQoS

QoS in MQTT refers to the level of guarantee for message delivery between the publisher and the subscriber. It can be `0`, `1` or `2` ([learn more](https://www.hivemq.com/blog/mqtt-essentials-part-6-mqtt-quality-of-service-levels/)).

## Name

A String indentifier that is intended to be both human and machine-readable.

## Number

Decimal number.

## String
A chain of UTF-8 characters.

## Version

Specifies a system version. Version must follow the Semantic Versioning, which means, it consists of three numbers separated by dots. These numbers represent the `MAJOR`, `MINOR` and `PATCH` version. An example: 5.0.0