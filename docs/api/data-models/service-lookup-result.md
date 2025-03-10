# ServiceLookupResult

Field | Type | Description
--- | --- | ---
instanceId | [Name](../primitives.md#name) | Unique identifier of the service instance.
provider | [SystemDescriptor](../data-models/system-descriptor.md) | Information about the service instance provider system.
serviceDefinition | [ServiceDefinitionDescriptor](../data-models/service-definition-descriptor.md) | Information about the service definition.
version | [Version](../primitives.md#version) | Version of the service instance.
expiresAt | [DateTime](../primitives.md#datetime) | The moment of the future from which the service instance will not be available.
metadata | [Metadata](../data-models/metadata.md) | Additional information about the service instance.
interfaces | List<[ServiceInterfaceDescriptor](../data-models/service-interface-descriptor.md)> | Available access interfaces of the service instance.
createdAt | [DateTime](../primitives.md#datetime) | Service instance was registered at this timestamp.
updatedAt | [DateTime](../primitives.md#datetime) | Service instance was modified at this timestamp.