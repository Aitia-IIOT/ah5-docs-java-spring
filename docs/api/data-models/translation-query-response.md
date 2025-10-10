# TranslationQueryResponse

Field | Type | Description
--- | --- | --- 
bridgeId | [TranslationBridgeID](../primitives.md#translationbridgeid) | Unique identifier of the translation bridge.
status | [TranslationBridgeStatus](../primitives.md#translationbridgestatus) | Status of the translation bridge.
usageReportCount | [Number](../primitives.md#number) | Information about how many times the translation bridge was used.
alivesAt | [DateTime](../primitives.md#datetime) | The moment when the translation bridge was used last time.
message | [String](../primitives.md#string) | Error message if the translation bridge is in error state.
consumer | [SystemName](../primitives.md#systemname) | The translation bridge is constructed for this consumer.
provider | [SystemName](../primitives.md#systemname) | The translation bridge is constructed to use this system.
serviceDefinition | [ServiceName](../primitives.md#servicename) | The translation bridge is constructed to use this service.
operation | [ServiceOperationName](../primitives.md#serviceoperationname) | The translation bridge is constructed to use this operation.
interfaceTranslator | [SystemName](../primitives.md#systemname) | The translation bridge is using this system as interface translator.
interfaceTranslatorData | [TranslationInterfaceTranslation- DataDescriptor](../data-models/translation-interface-translation-data-descriptor.md) | Additional information about the interface translator.
inputDataModelTranslator | [SystemName](../primitives.md#systemname) | The translation bridge is using this system as input data model translator.
inputDataModelTranslatorData | [TranslationDataModelTranslation- DataDescriptor](../data-models/translation-data-model-translation-data-descriptor.md) | Additional information about the data model translator for input translation.
outputDataModelTranslator | [SystemName](../primitives.md#systemname) | The translation bridge is using this system as output data model translator.
outputDataModelTranslatorData | [TranslationDataModelTranslation- DataDescriptor](../data-models/translation-data-model-translation-data-descriptor.md) | Additional information about the data model translator for output translation.
createdBy | [SystemName](../primitives.md#systemname) | The system that requested the translation bridge construction.
createdAt | [DateTime](../primitives.md#datetime) | Translation bridge was constructed at this timestamp.
updatedAt | [DateTime](../primitives.md#datetime) | Translation bridge was modified at this timestamp.
