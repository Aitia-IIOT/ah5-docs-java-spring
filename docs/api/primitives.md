# Primitives

## Address

A string representation of a network address. An address can be a version 4 IP address, a version 6 IP address, DNS name or MAC address.

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

A String indentifier that is intended to be both human and machine-readable. The allowed characters are letters, numbers and dash (`-`). A name has to start with a letter and cannot end with a dash. There are different restrictions on the length of the name depending on the data model.

## Number

Decimal number.

## PropertyValidator

An identifier of any suitable validator function chosen by the implementor of service. The validators have different kinds of inputs, depending on what to validate. Some validators have optional or mandatory **arguments**. The implemented validators are the following:

- `NOT_EMPTY_ADDRESS_LIST`: The **input** is a list of [Addresses](#address) and must contain at least one element. The validator checks if the addresses are valid and in alignment with the supported address types and performs normalization. An example for usage: validating HTTP interface access addresses.
- `NOT_EMPTY_STRING_SET`: The **input** is a list of [Strings](#string) and must contain at least one element. The validator checks if the are not empty and performs normalization. It can be used with the optional `NAME` **argument**. In that case, normalization and validation happens according to the naming convention. An example for usage: validating possible MQTT interface operations.
- `PORT`: This validator is meant to be used for ports. The **input** is an integer that represents a port number. The validator checks if the given value is between 1 and 65535, since in practice all valid port numbers fall within this range.
- `MINMAX`: The **input** is a [Number](#number). It is mandatory to give two [Strings](#string) as **arguments**, that represent the lower and upper limits of a closed interval. The validator checks if the input falls within the interval. (The `PORT` validator is a specific `MINMAX` validator, where the arguments are "1" and "65535".)
- `HTTP_OPERATIONS`: This validator is meant to be used for HTTP operations. The **input** is one or more key-value pair where the keys are [Strings](#string) and the values are also key-value pairs, where the keys are _path_ and _method_. A concrete example:
    ```
            "query-temperature": {
                "path": "/query",
                "method": "GET"
            },
            "set-temperature": {
                "path": "/set",
                "method": "PUT"
            }
    ```
    The validation and normalization happens according to the HTTP standards.

  
## Protocol

A string representation of a communication protocol. Examples: _http_, _https_, _tcp_, _ssl_...

## SecurityPolicy

Any suitable security policy chosen by the implementor of the service. The possible values are: `NONE`, `CERT_AUTH`, `TIME_LIMITED_TOKEN_AUTH`, `USAGE_LIMITED_TOKEN_AUTH`, `JSON_WEB_TOKEN_AUTH`.

## ServiceInstanceID

A string identifier of a service instance. It consists of the instance's provider name, service definition and version, each separated by double colons, as follows: `<provider-name>::<service-definition>::<version>`. An example for a valid service instance ID: _alert-provider1::alert-service1::1.0.0_. (Here the provider name is _alert-provider1_, the service definition is _alert-service1_, and the version is _1.0.0_.)

## String

A chain of UTF-8 characters.

## UUID

A UUID (Universally Unique Identifier) is a 128-bit identifier used to uniquely distinguish objects in distributed systems (e.g.: `550e8400-e29b-41d4-a716-446655440000`)

## Version

Specifies a system version. Version must follow the Semantic Versioning, which means, it consists of three numbers separated by dots. These numbers represent the `MAJOR`, `MINOR` and `PATCH` version. An example: 5.0.0