# Migration Guide from v4 to v5

**5th generation of Arrowhead FW  is not backward compatible with the prior vesions!**

## General

### Security & System Authentication

:fontawesome-solid-circle-exclamation: In version 4, users could choose between an insecure and secure Local Cloud depolyment. In a secure cloud, application systems authenticated their identities with X.509 certificates, and network communication was encrypted also with the same certificates. Now in version 5, authenticaction and secure communication is separated.
> Learn more about [v5 authentication policies](../api/authentication_policy.md).

#### I had an insecure cloud

:fontawesome-solid-gears: In case you maintained an insercure Local Cloud, now to achive the same, you need to set the [authentication.policy](../general/general_config_props.md) configuartion property to `declared`and the [server.ssl.enabled](../general/general_config_props.md) to `false`  in every Core and Support system.

:fontawesome-solid-gears: In case your application system was part of an insecure Local Cloud, now you need to update your application to apply the [declared](../api/authentication_policy.md#declared-http) authentication policy.

#### I had a secure cloud

:fontawesome-solid-gears: In case you maintained a sercure Local Cloud, now to achive the same, you need to set the [authentication.policy](../general/general_config_props.md) configuartion property to `certificate`, the [server.ssl.enabled](../general/general_config_props.md) to `true` and the [server.ssl.client-auth](../general/general_config_props.md) to `need` in every Core and Support system. Also, the Core and Support system's cetificates have to apply the new [certificate profiles](../help/certificate-profiles.md).

:fontawesome-solid-gears: In case your application system was part of a secure Local Cloud, now you need to have new certificate that apply the [new system certificate profile](../help/certificate-profiles.md#system-profile).

### Entity identifiers

:fontawesome-solid-circle-exclamation: In version 5, Core and Support systems use separate databases. In contrast, version 4 relied on a single shared database for both Core and Support systems. In version 4, entities (e.g., systems, service instances) were referenced by numeric database record IDs. This referencing mechanism is no longer supported in version 5, where cloud-wide unique, human-readable names and composite identifiers are used.

### Naming convention

:fontawesome-solid-circle-exclamation: Arrowehad v5 introduces a new [naming convetion](../help/naming-convention.md).

:fontawesome-solid-gears: At both the cloud operation and application system levels, the given names must be updated to follow the convention.

## ServiceRegistry

:fontawesome-solid-circle-exclamation: In v4, a service corresponded to a single operation (or a single endpoint, in other words). In v5, the service representation has been changed to follow the service–operation pattern, so one [service](../help/definitions.md#microservice-or-service) corresponds to a set of [operations](../help/definitions.md#service-operation) (or a set of endpoints, in other words).

:fontawesome-solid-circle-exclamation: In v4, the system and service access address was always the same. In v5 system address and service access address is separated.

:fontawesome-solid-circle-exclamation: In v4, the service interface could only be referenced. In v5, the service interface can be detailed.

:fontawesome-solid-circle-exclamation: In v4, the service version was only a single number. In v5, the service version must follow the semantic versioning.

:fontawesome-solid-circle-exclamation: In v4, the service metadata was String-String key-value pairs. In v5, the service metadata is String-Object key-value pairs allowing to provide metada in complex structures.

:fontawesome-solid-circle-exclamation: In v4, the security policy cloud be defined on service instance level. In v5, the security policy has to be defined on service instance interface level.

:fontawesome-solid-circle-exclamation: In v4, only one type of `TOKEN` security policy was available. In v5, [multiple token related security policies](../api/primitives.md#securitypolicy) are available.

:fontawesome-solid-gears: Application systems need to be updated to use the operations of [serviceDiscovery](../core_systems/service_registry.md#servicediscovery):

- Update your application system to use the [register](../api/serviceregistry/service-discovery-generic-http.md#register) service-operation replacing the `service-register` service used in v4. (System registration must precede service registration.)
- Update your application system to use the [revoke](../api/serviceregistry/service-discovery-generic-http.md#revoke) service-operation replacing the `service-unregister` service used in v4.
- Update your application system to use the [lookup](../api/serviceregistry/service-discovery-generic-http.md#lookup) service-operation replacing the `query` service used in v4.

:fontawesome-solid-gears: Application systems need to be updated to use the operations of [systemDiscovery](../core_systems/service_registry.md#systemdiscovery):

- Update your application system to use the [register](../api/serviceregistry/system-discovery-generic-http.md#register) service-operation replacing the `register-system` and/or `service-register` service used in v4.

- Update your application system to use the [revoke](../api/serviceregistry/system-discovery-generic-http.md#revoke) service-operation replacing the `unregister-system` service used in v4.

:fontawesome-solid-gears: In case of your application system required `TOKEN` security, now to require the same type of token, use `RSA_SHA256_JSON_WEB_TOKEN_AUTH` or `RSA_SHA512_JSON_WEB_TOKEN_AUTH` policy. 

## SystemRegistry

:fontawesome-solid-circle-exclamation: The services of SystemRegistry Support System have been merged into ServiceRegistry Core System. 

:fontawesome-solid-gears: Application systems need to be updated to use the operations of [systemDiscovery](../core_systems/service_registry.md#systemdiscovery):

- Update your application system to use the [register](../api/serviceregistry/system-discovery-generic-http.md#register) service-operation replacing the `register` service used in v4.

- Update your application system to use the [revoke](../api/serviceregistry/system-discovery-generic-http.md#revoke) service-operation replacing the `unregister` service used in v4.

- Update your application system to use the [lookup](../api/serviceregistry/system-discovery-generic-http.md#lookup) service-operation replacing the `query` service used in v4.

## DeviceRegistry

:fontawesome-solid-circle-exclamation: The services of DeviceRegistry Support System have been merged into ServiceRegistry Core System. 

:fontawesome-solid-gears: Application systems need to be updated to use the operations of [deviceDiscovery](../core_systems/service_registry.md#devicediscovery):

- Update your application system to use the [register](../api/serviceregistry/device-discovery-generic-http.md#register) service-operation replacing the `register` service used in v4.

- Update your application system to use the [revoke](../api/serviceregistry/device-discovery-generic-http.md#revoke) service-operation replacing the `unregister` service used in v4.

- Update your application system to use the [lookup](../api/serviceregistry/device-discovery-generic-http.md#lookup) service-operation replacing the `query` service used in v4.
