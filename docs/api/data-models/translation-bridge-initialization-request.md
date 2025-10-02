# TranslationBridgeInitializationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
bridgeId | [TranslationBridgeID](../primitives.md#translationbridgeid) | yes | The given identifier of the intended translation bridge.
operation | [ServiceOperationName](../primitives.md#serviceoperationname) | yes | The target service-operation.
inputInterface | [InterfaceName](../primitives.md#interfacename) | yes | The name of the interface template to be translated.
targetInterface | [InterfaceName](../primitives.md#interfacename) | yes | The name of the interface template of the target serviceoperation.
targetInterfaceProperties | [Metadata](./metadata.md) | yes | Interface specific data of the target service-operation.
inputDataModelRequirement | [DataModelID](../primitives.md#datamodelid) | no | The data model identifier of what the consumer can provide as the target serviceoperation input.
inputDataModelTranslator | [DataModelTranslationDescriptor](./data-model-translation-descriptor.md) | no | The data model translation provider for translating the input of the target serviceoperation.
resultDataModelRequirement | [DataModelID](../primitives.md#datamodelid) | no | The data model identifier of what the consumer can handle as the target serviceoperation result.
outputDataModelTranslator | [DataModelTranslationDescriptor](./data-model-translation-descriptor.md) | no | The data model translation provider for translating the output of the target serviceoperation.
authorizationToken | [AccessToken](../primitives.md#accesstoken) | no | Authorization token for the target service-operation if required.
interfaceTranslatorSettings | [Metadata](./metadata.md) | no | Any configuration data that might be required by the interface translation process.