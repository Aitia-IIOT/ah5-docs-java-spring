#  InterfaceTemplateRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
name | [InterfaceName](../primitives.md#interfacename) | yes | Unique name of the interface template.
protocol | [Protocol](../primitives.md#protocol) | yes | Protocol of the interface template.
propertyRequirements | List<[InterfaceTemplatePropertyRequest](../data-models/interface-template-property-request.md)> | yes | Properties of the interface template.