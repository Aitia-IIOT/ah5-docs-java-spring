# TranslationDiscoveryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
candidates | List<[TranslationDiscovery- ServiceInstanceDescriptor](../data-models/translation-discovery-service-instance-descriptor.md)> | yes | Possible service instances for consuming via a translation bridge.
operation | [ServiceOperationName](../primitives.md#serviceoperationname) | yes | The operation that the requester wants to consume.
interfaceTemplateNames | List<[InterfaceName](../primitives.md#interfacename)> | yes | The name of the interfaces that the consumer can use.
inputDataModelId | [DataModelID](../primitives.md#datamodelid) | yes (no) | The identifier of the data model that the requester can use as input payload of the specified operation. Must be omitted if there is no input payload.
outputDataModelId | [DataModelID](../primitives.md#datamodelid) | yes (no) | The identifier of the data model that the requester can use as response payload of the specified operation. Must be omitted if there is no response payload.
