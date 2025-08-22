# DeviceLookupResult

Field | Type | Description
--- | --- | --- 
name | [DeviceName](../primitives.md#devicename) | Unique identifier of the device.
metadata | [Metadata](../data-models/metadata.md) | Additional information about the device.
addresses | List<[AddressDescriptor](../data-models/address-descriptor.md)> | Different kind of addresses of the device.
createdAt | [DateTime](../primitives.md#datetime) | Device was registered at this timestamp.
updatedAt | [DateTime](../primitives.md#datetime) | Device was modified at this timestamp.