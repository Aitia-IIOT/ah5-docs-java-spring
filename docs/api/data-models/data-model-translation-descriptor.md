# DataModelTranslationDescriptor

Field | Type | Mandatory | Description
--- | --- | --- | ---
fromModelId | [DataModelID](../primitives.md#datamodelid) | yes | The identifier of the data model to be translated.
toModelId | [DataModelID](../primitives.md#datamodelid) | yes | The identifier of the required result data model.
interfaceProperties | [Metadata](./metadata.md) | yes | Interface specific data of the model translation provider.
configurationSettings | [Metadata](./metadata.md) | no | Any configuration data that might be required by the model translation provider.
