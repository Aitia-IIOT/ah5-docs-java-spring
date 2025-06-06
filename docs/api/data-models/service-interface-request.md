# ServiceInterfaceRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
templateName | [InterfaceName](../primitives.md#interfacename) | yes | The name of the interface template that describes the interface structure.
protocol | [Protocol](../primitives.md#protocol) | no (yes) | The communication protocol of the interface. Only mandatory if the interface template is not previously known in the Local Cloud.
policy | [SecurityPolicy](../primitives.md#securitypolicy) | yes | The security of the interface.
properties | [Metadata](../data-models/metadata.md) | yes | Interface template-specific data.