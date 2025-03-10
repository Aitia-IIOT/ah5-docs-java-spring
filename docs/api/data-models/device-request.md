# DeviceRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
name | [Name](../primitives.md#name) | yes | Unique identifier of the device.
metadata | [Metadata](../data-models/metadata.md) | no | Additional information about the device.
addresses | List<[Address](../primitives.md#address)> | yes | Different kinds of addresses of the device.