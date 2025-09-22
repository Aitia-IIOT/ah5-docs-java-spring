# Service Security

Secured [service](./definitions.md#microservice-or-service) providing is a key in a loosley-coupled, System-Of-Systems environment. Arrowhead 5 offers multiple security levels to be applied by the service providers.

**Authorization Rules:**<br />
Specifies which systems have what permissions to which service instances.
> Learn more about the [authorization rules](../core_systems/authorization.md).

**Security Policies:**<br />
Specifies how to prove access rights when consuming a service.
> Learn more about the [security policies](#security-policies).

**Communication Profiles:**<br />
Among other things, specifies the required encryption for data exchange and the method of providing service access credentials, like certificates or tokens.
> Learn more about the [communication profiles](../api/communication-profiles/communication-profiles-overview.md)

## Security Policies

The Service Security Policy must be defined during service registration for each [interface](./definitions.md#service-interface) of the given [service](./definitions.md#microservice-or-service).

> See the **register** operation of [serviceDiscovery](../core_systems/service_registry.md#servicediscovery) service.

### Insecure

#### NONE

If `NONE` security policy is applied:

- Service consumption attempts SHALL NOT be verified on the provider side.
- Consumers SHALL NOT be required to attach any access credentials to their [service operation](./definitions.md#service-operation) requests.

### Certificated based

#### CERT_AUTH

If `CERT_AUTH` security policy is applied:

- Service consumption attempts SHALL be verified on the provider side.
- An Arrowhead-compliant [system profile certificate](./certificate-profiles.md#system-profile) SHALL be attached to the [service operation](./definitions.md#service-operation) request by the consumer systems.
- Based on the certificate, the provider system SHALL verify whether the consumer belongs to the same [Local Cloud](./definitions.md#local-cloud) or to any other **trusted** cloud.

### Token based

#### TIME_LIMITED_TOKEN_AUTH

This type of token expires within a given time.

If `TIME_LIMITED_TOKEN_AUTH` security policy is applied:

- Service consumption attempts SHALL be verified on the provider side.
- A time-limited [access token](./definitions.md#access-token), issued by the ConsumerAuthorization Core System, SHALL be included in the [service operation](./definitions.md#service-operation) request by the consumer systems.
- The provider SHALL verify the given access token with the ConsumerAuthorization Core System by consuming the **verify** operation of the [authorizationToken](../core_systems/authorization.md#authorizationtoken) service.
- Based on the access token verification result, the provider SHALL verify whether the token scope covers the attempted service operation. 

#### USAGE_LIMITED_TOKEN_AUTH

This type of token expires after a certain number of uses.

If `USAGE_LIMITED_TOKEN_AUTH` security policy is applied:

- Service consumption attempts SHALL be verified on the provider side.
- A usage-limited [access token](./definitions.md#access-token), issued by the ConsumerAuthorization Core System, SHALL be included in the [service operation](./definitions.md#service-operation) request by the consumer systems.
- The provider SHALL verify the given access token with the ConsumerAuthorization Core System by consuming the **verify** operation of the [authorizationToken](../core_systems/authorization.md#authorizationtoken) service.
- Based on the access token verification result, the provider SHALL verify whether the token scope covers the attempted service operation. 

#### BASE64_SELF_CONTAINED_TOKEN_AUTH

This type of token expires within a given time and holds a payload.

If `BASE64_SELF_CONTAINED_TOKEN_AUTH` security policy is applied:

- Service consumption attempts SHALL be verified on the provider side.
- A self-contained [access token](./definitions.md#access-token), issued by the ConsumerAuthorization Core System, SHALL be included in the [service operation](./definitions.md#service-operation) request by the consumer systems.
- The provider SHALL verify the given access token itself by [Base64](https://en.wikipedia.org/wiki/Base64) decoding it with [ISO 8859-1](https://en.wikipedia.org/wiki/ISO/IEC_8859-1) charset and reading the payload content.
- Based on the payload content, the provider SHALL verify whether
    - the token is not expired,
    - the token is issued for its system name,
    - the token is issued for the attempted service,
    - the token scope covers the attempted service operation.
- The provider MAY register an access token encryption key at its start-up by consuming the **register-encryption-key** operation of the [authorizationToken](../core_systems/authorization.md#authorizationtoken) service in order to make every access token encrypted that will be issued for it.
    - In this case provider SHALL decrypt the access token first with the same key that was registered into the ConsumerAuthorization Core System.

_Token payload structure:_

```
<consumer-cloud-name>|<consumer-name>|<provider-name>|<service-definition>|<scope>|SERVICE-DEF|<expiration-time-ISO8601>
```

_Token payload example:_

```
LOCAL|TemperatureConsumer|TemperatureProvider|kelvinInfo|query-temperature|SERVICE-DEF|2025-06-23T08:35:43.217717900Z
```

#### RSA_SHA256_JSON_WEB_TOKEN_AUTH

This type of token expires within a given time, holds a header, a payload and is signed by ConsumerAuthorization Core System using RSA-SHA256 algorithm. Available only, when Core/Support systems are SSL enabled.

If `RSA_SHA256_JSON_WEB_TOKEN_AUTH` security policy is applied:

- Service consumption attempts SHALL be verified on the provider side.
- A self-contained [access token](./definitions.md#access-token), issued by the ConsumerAuthorization Core System, SHALL be included in the [service operation](./definitions.md#service-operation) request by the consumer systems.
- The provider SHALL verify the given access token itself by [decoding it](https://www.jwt.io/introduction#difference-decoding-encoding-jwt), reading the header and payload contents and verifying the signature.
- Provider SHALL obtain the public key of ConsumerAuthorization Core System by consuming the **get-public-key** operation of [authorizationToken](../core_systems/authorization.md#authorizationtoken) in order to perform the signature verification.
    - This should be done once, at system start-up.
- Based on the payload content, the provider SHALL verify whether
    - the token is not expired,
    - the token is issued for its system name,
    - the token is issued for the attempted service,
    - the token scope covers the attempted service operation.
- The provider MAY register an access token encryption key at its start-up by consuming the **register-encryption-key** operation of the [authorizationToken](../core_systems/authorization.md#authorizationtoken) service in order to make every access token encrypted that will be issued for it.
    - In this case provider SHALL decrypt the access token first with the same key that was registered into the ConsumerAuthorization Core System.

_Token header example:_

```
{
  "typ": "JWT",                     -> type of token
  "alg": "RS256"                    -> signature algorithm identifier
}
```

_Token payload example:_

```
{
  "jti": "H5dsQ0sbA_JJmSKJxfcJjg",  -> unique identifier
  "iss": "ConsumerAuthorization",   -> issuer name
  "iat": 1758144278,                -> issued at (unix epoch)
  "nbf": 1758144218,                -> not valid before (unix epoch)
  "exp": 1758144338,                -> expiration time (unix epoch)
  "psn": "TemperatureProvider",     -> provider system name
  "csn": "TemperatureConsumer",     -> consumer system name
  "ccn": "LOCAL",                   -> consumer cloud name
  "tat": "SERVICE_DEF",             -> target type
  "tan": "kelvinInfo",              -> target name (service definition)
  "sco": "query-temperature"        -> scope (not defined if token scope covers all the service operations)
}
```

#### RSA_SHA512_JSON_WEB_TOKEN_AUTH

This token is exactly the same as `RSA_SHA256_JSON_WEB_TOKEN_AUTH`, except the signing algorithm which is **RSA-SHA512** (id: `RS512`), therefore signature verification is slightly slower, but the token itself is stronger.

#### TRANSLATION_BRIDGE_TOKEN_AUTH

This token type is NOT applicable during regular service registrations. This token type is issued by the TranslationManager Support System, when a translation bridge is created between a consumer and a provider. Consumer SHALL include this token in its [service operation](./definitions.md#service-operation) request when the service consumption happens with translation.