# InterfacePropertyDescriptor

Field | Type | Description
--- | --- | ---
name | [String](../primitives.md#string) | Name of the property.
mandatory | [Boolean](../primitives.md#boolean) | True if the property is mandatory, false if optional.
validator | [PropertyValidator](../primitives.md#propertyvalidator) | Name of the validator assigned to the property.
validatorParams | List<[String](../primitives.md#string)> | Parameter values of the validator (if needed).