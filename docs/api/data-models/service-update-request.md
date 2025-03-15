# ServiceUpdateRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
instanceId | [Name](../primitives.md#name) | yes | The unique identifier of the service instance.
expiresAt | [DateTime](../primitives.md#datetime) | no | The moment of the future from which the service instance will not be available.
metadata | [Metadata](../data-models/metadata.md) | no | Additional information about the service instance.
interfaces | List<[ServiceInterfaceRequest](../data-models/service-interface-request.md)> | yes | Available access interfaces of the service instance.