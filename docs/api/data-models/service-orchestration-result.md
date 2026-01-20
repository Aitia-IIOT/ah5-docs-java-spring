# ServiceOrchestrationResult

Field | Type | Description
--- | --- | ---
serviceInstanceId | [ServiceInstanceID](../primitives.md#serviceinstanceid) | Unique identifier of the service instance.
providerName | [SystemName](../primitives.md#systemname) | Unique identifier of the provider system.
serviceDefinition | [ServiceName](../primitives.md#servicename) | Unique identifier of the service definition.
version | [Version](../primitives.md#version) | Version of the service instance.
cloudIdentitifer | [CloudIdentifier](../primitives.md#cloudidentifier) | Unique identifier of the provider cloud.
aliveUntil | [DateTime](../primitives.md#datetime) | The service instance is available until this time. Not supported by **simple-store** strategy.
exclusiveUntil | [DateTime](../primitives.md#datetime) | The service instance is reserved until this time. Not supported by **simple-store** strategy.
metadata | [Metadata](../data-models/metadata.md) | Additional information about the service instance. Not supported by **simple-store** strategy.
interfaces | List<[ServiceInterfaceDescriptor](../data-models/service-interface-descriptor.md)> | Available access interfaces of the service instance. Not supported by **simple-store** strategy.
authorizationToken | [AuthorizationTokenMap](../data-models/authorization-token-map.md) | Generated authorization tokens if any. Not supported by **simple-store** strategy.