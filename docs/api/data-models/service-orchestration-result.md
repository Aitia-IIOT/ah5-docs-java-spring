# ServiceOrchestrationResult

Field | Type | Description
--- | --- | ---
serviceInstanceId | [ServiceInstanceID](../primitives.md#serviceinstanceid) | Unique identifier of the service instance.
providerName | [Name](../primitives.md#name) | Unique identifier of the provider system.
serviceDefinition | [Name](../primitives.md#name) | Unique identifier of the service definition.
version | [Version](../primitives.md#version) | Version of the service instance.
cloudIdentitifer | [Name](../primitives.md#name) | Unique identifier of the provider cloud.
aliveUntil | [DateTime](../primitives.md#datetime) | The service instance is available until this time.
exclusiveUntil | [DateTime](../primitives.md#datetime) | The service instance is reserved until this time.
metadata | [Metadata](../data-models/metadata.md) | Additional information about the service instance.
interfaces | List<[ServiceInterfaceDescriptor](../data-models/service-interface-descriptor.md)> | Available access interfaces of the service instance.