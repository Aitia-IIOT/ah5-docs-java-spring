# DeviceResult

Field | Type | Description
--- | --- | ---
name | [Name](../primitives.md#name) | Unique identifier of the device.
metadata | [Metadata](../data-models/metadata.md) | Additional information about the device.
addresses | List<[AddressDescriptor](../data-models/address-descriptor.md)> | Different kinds of addresses of the device.
createdAt | [DateTime](../primitives.md#datetime) | Device was registered at this timestamp.
updatedAt | [DateTime](../primitives.md#datetime) | Device was modified at this timestamp.