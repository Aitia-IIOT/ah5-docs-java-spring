# DataModelTranslationInitRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
inputModelId | [DataModelID](../primitives.md#datamodelid) | yes | The identifier of the data model to be translated.
outputModelId | [DataModelID](../primitives.md#datamodelid) | yes | The identifier of the required result data model.
payload | [String](../primitives.md#string) | yes | Base64 encoded byte array (UTF-8 charset) of the input data payload to be translated.
settings | [KeyValuePair](../primitives.md#keyvaluepair) | no | Any configuration data that might be required by a translation process.