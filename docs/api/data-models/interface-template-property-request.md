#  InterfaceTemplatePropertyRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
name | [String](../primitives.md#string) | yes | Name of the property.
mandatory | [Boolean](../primitives.md#boolean) | yes | True if the property is mandatory, false if optional.
validator | [PropertyValidator](../primitives.md#propertyvalidator) | no | Name of the validator assigned to the property.
validatorParams | List<[String](../primitives.md#string)> | no | Parameter values of the validator.