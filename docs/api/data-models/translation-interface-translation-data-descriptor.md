# TranslationInterfaceTranslationDataDescriptor

Field | Type | Description
--- | --- | ---
fromInterfaceTemplate | [InterfaceName](../primitives.md#interfacename) | Translator accepts requests using this interface.
toInterfaceTemplate | [InterfaceName](../primitives.md#interfacename) | Translator sends requests to target provider using this interface.
token | [AccessToken](../primitives.md#accesstoken) | An authorization token for the target operation (if required).
interfaceProperties | [Metadata](../data-models/metadata.md) | Access interface data of the interface translator.
configurationSettings | [Metadata](../data-models/metadata.md) | Additional configuration of the interface translator.