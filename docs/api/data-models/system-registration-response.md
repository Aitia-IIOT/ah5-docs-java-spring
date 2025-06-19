# SystemRegistrationResponse

Field | Type | Description
--- | --- | --- 
name | [SystemName](../primitives.md#systemname) | Unique identifier of the registered system.
metadata | [Metadata](../data-models/metadata.md) | Additional information about the registered system.
version | [Version](../primitives.md#version) | Version of the registered system.
addresses | List<[AddressDescriptor](../data-models/address-descriptor.md)> | Different kind of addresses of the registered system.
device | [DeviceDescriptor](../data-models/device-descriptor.md) | Information about the device on which the system is running.
createdAt | [DateTime](../primitives.md#datetime) | System was registered at this timestamp.
updatedAt | [DateTime](../primitives.md#datetime) | System was modified at this timestamp.