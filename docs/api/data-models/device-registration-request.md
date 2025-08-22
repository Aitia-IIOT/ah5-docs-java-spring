# DeviceRegistrationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
name | [DeviceName](../primitives.md#devicename) | yes | Unique identifier of the device.
metadata | [Metadata](../data-models/metadata.md) | no | Additional information about the device.
addresses | List<[Address](../primitives.md#address)> | yes | Different kind of addresses of the device.