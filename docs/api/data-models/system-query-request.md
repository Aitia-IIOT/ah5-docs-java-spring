# SystemQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried systems.
systemNames | List<[Name](../primitives.md#name)> | no | Requester is looking for systems with any of the specified names.
addresses | List<[Address](../primitives.md#address)> | no | Requester is looking for systems with any of the specified addresses.
addressType | [AddressType](../primitives.md#addresstype) | no | Requester is looking for systems with the specified type of address.
metadataRequirementsList | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | Requester is looking for systems that are matching any of the specified metadata requirements.
versions | List<[Version](../primitives.md#version)> | no | Requester is looking for systems with any of the specified versions.
deviceNames | List<[Name](../primitives.md#name)> | no | Requester is looking for systems that are running on any of the specified devices.