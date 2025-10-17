# TranslationNegotiationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
bridgeId | [TranslationBridgeID](../primitives.md#translationbridgeid) | no | The unique identifier of the results of a previously executed discovery operation.
target | [TranslationNegotiation- ServiceInstanceDescriptor](../data-models/translation-negotiation-service-instance-descriptor.md) | yes | Information about the target service instance.
operation | [ServiceOperationName](../primitives.md#serviceoperationname) | no (yes) | The operation that the requester wants to consume. Mandatory if _bridgeId_ is not specified.
interfaceTemplateName | [InterfaceName](../primitives.md#interfacename) | no (yes) | The name of the interface that the consumer can use. Mandatory if _bridgeId_ is not specified.
inputDataModelId | [DataModelID](../primitives.md#datamodelid) | no (yes) | The identifier of the data model that the requester can use as input payload of the specified operation. Can be omitted if _bridgeId_ is specified and must be omitted if there is no input payload.
outputDataModelId | [DataModelID](../primitives.md#datamodelid) | no (yes) | The identifier of the data model that the requester can use as response payload of the specified operation. Can be omitted if _bridgeId_ is specified and must be omitted if there is no response payload.
