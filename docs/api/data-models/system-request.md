# SystemRequest

# SystemRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
name | [Name](../primitives.md#name) | yes | The name of the system.
metadata | [Metadata](../data-models/metadata.md) | no | Additional information about the system.
version | [Version](../primitives.md#version) | no | Version of the system.
addresses | List<[Address](../primitives.md#address)> | yes | Different kinds of addresses of the system.
deviceName | [Name](../primitives.md#name) | no | Unique identifier of the device on which the system is running.
