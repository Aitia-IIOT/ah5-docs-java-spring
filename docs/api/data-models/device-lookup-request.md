# DeviceLookupRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
deviceNames | List<[DeviceName](../primitives.md#devicename)> | no | Requester is looking for devices with any of the specified names.
addresses | List<[Address](../primitives.md#address)> | no | Requester is looking for devices with any of the specified addresses.
addressType | [AddressType](../primitives.md#addresstype) | no | Requester is looking for devices with the specified type of address.
metadataRequirementsList | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | Requester is looking for devices that are matching any of the specified metadata requirements