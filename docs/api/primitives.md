# Primitives

## AccessToken

A random and unique **String** of characters that is issued for a beneficiary system and is associated with a target, a target system, a scope and is expiring.

## AccessTokenVariant

**String** value that specifies an exact token technology variant. The possible values are `TIME_LIMITED_TOKEN_AUTH`, `USAGE_LIMITED_TOKEN_AUTH`, `BASE64_SELF_CONTAINED_TOKEN_AUTH`, `RSA_SHA256_JSON_WEB_TOKEN_AUTH`, `RSA_SHA512_JSON_WEB_TOKEN_AUTH`.

## Address 

A **String** representation of a network address. An address can be a version 4 IP address, a version 6 IP address, DNS name or MAC address.

## AddressType

**String** value of a network address type. Could be only `HOSTNAME`, `IPV4`, `IPV6` or `MAC`.

## AuthenticationMethod

A **String** representation of an authentication method. Currently, only `PASSWORD` is supported.`

## AuthorizationLevel

A **String** representation of an authorization policy's priority level. Could be only `PR` (for policies that were created by their own providers), or `MGMT` (for policies that were created by a higher entity).

## AuthorizationPolicyInstanceID

A composite **String** identifier that is intended to be both human and machine-readable. It consists of the instance’s level (`PR` for provider and `MGMT` for management), cloud identifier (or the word `LOCAL` in case of the Local Cloud), provider name, target type and target, each separated by a pipe as follows: `<Level>|<CloudIdentifier>|<ProviderName>|<TargetType><Target>`. Each part must follow its related naming convention. An example for a valid policy instance ID: _PR|LOCAL|TemperatureProvider|SERVICE\_DEF|celsiusInfo_. 

## AuthorizationPolicyType

A **String** representation of the type of the authorization policy. Could be only `ALL` (everybody can use the target in the appropriate cloud), `WHITELIST` (whitelist-based policy), `BLACKLIST` (blacklist-based policy) or `SYS_METADATA` (system-level metadata-based policy).

## AuthorizationTargetType

A **String** representation of the type of target in authorization policies. Could be only `SERVICE_DEF` (for service definitions) or `EVENT_TYPE` (for event types).

## BlacklistReason

A chain of UTF-8 characters with a maximum length of 1024.

## Boolean

A **boolean** value, one out of `true` or `false`.

## CloudIdentifier

A **String** identifier of a Local Cloud. It consists of the cloud name and the organization name separated by a pipe, as follows: `<CloudName>|<OrganizationName>`. An example for a valid cloud identifier: _TestCloud|AitiaInc_. (Here the cloud name is _TestCloud_ and the organization name is _AitiaInc_.) In certain cases, the word `LOCAL` is also considered valid and it references the Local Cloud.

## DateTime

A **String** value that pinpoints a moment in time in the format of ISO8601 standard `yyyy-mm-ddThh:MM:ssZ`, where ”yyyy” denotes
year (4 digits), ”mm” denotes month starting from 01, ”dd” denotes day starting from 01, ”T” is the separator
between date and time part, ”hh” denotes hour in the 24-hour format (00-23), ”MM” denotes minute (00-59),
”ss” denotes second (00-59). ”Z” indicates that the time is in UTC. An example of a valid date/time string is
”2024-12-05T12:00:00Z”

## DeviceName

A **String** identifier that is intended to be both human and machine-readable. The allowed characters are uppercase letters (english alphabet only), numbers and underscore (\_). A name has to start with a letter, cannot end with an underscore and must follow the _UPPER\_SNAKE\_CASE_ naming convention. The identifier maximum length is 63 characters.

## Direction
The direction of a sorting operation. Possible values are the **String** representation of ascending (`ASC`) or descending (`DESC`) order.

## EncryptionAlgorithmName 

A **String** identifier that belongs to an encryption algorithm. Possible values are:
* `AES/ECB/PKCS5Padding`: encryption without any addition to the encryption key.
* `AES/CBC/PKCS5Padding`: encryption with a generated initialization vector as an addition to the encryption key.

## ErrorType

**String** value of the error type. Could be `ARROWHEAD`, `INVALID_PARAMETER`, `AUTH`, `FORBIDDEN`, `DATA_NOT_FOUND`,
`TIMEOUT`, `LOCKED`, `INTERNAL_SERVER_ERROR` or `EXTERNAL_SERVER_ERROR`.

## EventTypeName

A **String** identifier that is intended to be both human and machine-readable. The allowed characters are letters (english alphabet only) and numbers. A name has to start with a letter and must follow the camelCase naming convention. The identifier maximum length is 63 characters.

## IdentityToken

A unique and expiring **String** data issued by a central component to verify a system's identity during the interactions with the trusted Core and Support systems, allowing access without sharing the credentials with each and every Core/Support system at each and every interaction.  

## InterfaceName

A **String** identifier that is intended to be both human and machine-readable. The allowed characters are letters (english alphabet only), numbers and underscore (_). A name has to start with a letter and must follow the snake_case naming convention. The identifier maximum length is 63 characters.

## KeyValuePair

Association of a key of type [String](#string) and a value of any type. It is represented as a **Map<String, Object\>** Some examples:
```
{
    "verbose": true,
    "name": "TemperatureProvider1",
    "size": 500
}
```

## LogSeverity

Alias for a **String** value that describes the kind and seriousness of a log message. Could be `ALL`, `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR` or `FATAL`.

## Mode

Specifies whether the queried records should have the _active_ flag set. The possible values are: `ALL`, `ACTIVES`, `INACTIVES`.

## MQTTQoS

QoS in MQTT refers to the level of guarantee for message delivery between the publisher and the subscriber. It is represented as an **Integer**. It can be `0`, `1` or `2` ([learn more](https://www.hivemq.com/blog/mqtt-essentials-part-6-mqtt-quality-of-service-levels/)).

## Number

Decimal number, it is represented as **int/Integer** or **long/Long** depending on the range of possible values.

## PropertyValidator

A **String** identifier of any suitable validator function chosen by the implementor of service. The validators have different kinds of inputs, depending on what to validate. Some validators have optional or mandatory **String** *arguments*. The implemented validators are the following:

- `NOT_EMPTY_ADDRESS_LIST`: The **input** is a list of [Addresses](#address) and must contain at least one element. The validator checks if the addresses are valid and in alignment with the supported address types and performs normalization. An example for usage: validating HTTP interface access addresses.
- `NOT_EMPTY_STRING_SET`: The **input** is a list of [Strings](#string) and must contain at least one element. The validator checks if the are not empty and performs normalization. It can be used with the optional `OPERATION` **argument**. In that case, normalization and validation happens according to the naming convention. An example for usage: validating possible MQTT interface operations.
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

A **String** representation of a communication protocol. Examples: _http_, _https_, _tcp_, _ssl_...

## PublicKey

A Base64 **String** representation of the public byte array cryptographic key retrieved from an X.509 certificate.

## SecurityPolicy

A **String** representation of security policies. The possible values are: `NONE`, `CERT_AUTH`, `TIME_LIMITED_TOKEN_AUTH`, `USAGE_LIMITED_TOKEN_AUTH`, `BASE64_SELF_CONTAINED_TOKEN_AUTH`, `RSA_SHA256_JSON_WEB_TOKEN_AUTH`, `RSA_SHA512_JSON_WEB_TOKEN_AUTH`.

## ServiceInstanceID

A **String** identifier of a service instance. It consists of the instance's provider name, service definition and version, each separated by pipe, as follows: `<provider-name>|<service-definition>|<version>`. An example for a valid service instance ID: _AlertProvider1|alertService1|1.0.0_. (Here the provider name is _AlertProvider1_, the service definition is _alertService1_, and the version is _1.0.0_.)

## ServiceName
A **String** identifier that is intended to be both human and machine-readable. The allowed characters are letters (english alphabet only) and numbers. A name has to start with a letter and must follow the camelCase naming convention. The identifier maximum length is 63 characters.

## ServiceOperationName

A **String** identifier that is intended to be both human and machine-readable. The allowed characters are letters (english alphabet only), numbers and dash (-). A name has to start with a letter, cannot end with a dash and must follow the kebab-case naming convention. The identifier maximum length is 63 characters.

## ServiceOrchestrationJobStatus

Alias for a **String** value that describes the actual state of a job. Can be: `PENDING`, `IN_PROGRESS`, `DONE`, `ERROR`.

## ServiceOrchestrationType

Alias for a **String** value that can be only `PULL` or `PUSH`.

## String

A chain of UTF-8 characters.

## SystemName

A **String** identifier that is intended to be both human and machine-readable. The allowed characters are letters (english alphabet only) and numbers. A name has to start with a letter and must follow the _PascalCase_ naming convention. The identifier maximum length is 63 characters.

## TokenType

A **String** name that groups token technologies by usage characteristics. Can be `USAGE_LIMITED_TOKEN`, `TIME_LIMITED_TOKEN` or `SELF_CONTAINED_TOKEN`.

## UUID

A UUID (Universally Unique Identifier) is a 128-bit **String** identifier used to uniquely distinguish objects in distributed systems (e.g.: `550e8400-e29b-41d4-a716-446655440000`)

## Version

A **String** value that specifies a system or service instance version. Version must follow the Semantic Versioning, which means, it consists of three numbers separated by dots. These numbers represent the `MAJOR`, `MINOR` and `PATCH` version. An example: 5.0.0