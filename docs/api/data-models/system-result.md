# SystemResult

Field | Type | Description
--- | --- | ---
name | [Name](../primitives.md#name) | Unique identifier of the system.
metadata | [Metadata](../data-models/metadata.md) | Additional information about the system.
version | [Version](../primitives.md#version) | Version of the system.
addresses | List<[AddressDescriptor](../data-models/address-descriptor.md)> | Different kinds of addresses of the system.
device | [DeviceResult](../data-models/device-result.md) | Information about the device on which the system is running.
createdAt | [DateTime](../primitives.md#datetime) | System was registered at this timestamp.
updatedAt | [DateTime](../primitives.md#datetime) | System was modified at this timestamp.