# TranslationBridgeInitializationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
bridgeId | [TranslationBridgeID](../primitives.md#translationbridgeid) | yes | The given identifier of the intended translation bridge.
operation | [ServiceOperationName](../primitives.md#serviceoperationname) | yes | The target service operation.
inputInterface | [InterfaceName](../primitives.md#interfacename) | yes | The name of the interface template to be translated.
targetInterface | [InterfaceName](../primitives.md#interfacename) | yes | The name of the interface template of the target service operation.
targetInterfaceProperties | [Metadata](./metadata.md) | yes | Interface specific data of the target service operation.
inputDataModelRequirement | [DataModelID](../primitives.md#datamodelid) | no (yes) | The data model identifier of what the consumer can provide as the target service operation input. Mandatory if service operation input needs to be translated.
inputDataModelTranslator | [DataModelTranslationDescriptor](./data-model-translation-descriptor.md) | no | The data model translation provider for translating the input of the target service operation.
resultDataModelRequirement | [DataModelID](../primitives.md#datamodelid) | no (yes) | The data model identifier of what the consumer can handle as the target service operation result. Mandatory if service operation result needs to be translated.
resultDataModelTranslator | [DataModelTranslationDescriptor](./data-model-translation-descriptor.md) | no | The data model translation provider for translating the output of the target service operation.
authorizationToken | [AccessToken](../primitives.md#accesstoken) | no | Authorization token for the target service operation if required.
interfaceTranslatorSettings | [Metadata](./metadata.md) | no | Any configuration data that might be required by the interface translation process.