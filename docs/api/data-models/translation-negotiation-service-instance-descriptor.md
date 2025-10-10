# TranslationNegotiationServiceInstanceDescriptor

Field | Type | Mandatory | Description
--- | --- | --- | ---
instanceId | [ServiceInstanceID](../primitives.md#serviceinstanceid) | yes | Unique identifier of the service instance.
provider | [TranslationDiscoverySystemDescriptor](../data-models/translation-discovery-system-descriptor.md) | no (yes) | Provider system. Mandatory if the _bridgeId_ is not specified in the containing model.
serviceDefinition | [TranslationDiscoveryServiceDefinition- Descriptor](../data-models/translation-discovery-service-definition-descriptor.md) | no (yes) | Service definition. Mandatory if the _bridgeId_ is not specified in the containing model..
interfaces | List<[TranslationDiscovery- ServiceInstanceInterfaceDescriptor](../data-models/translation-discovery-service-instance-interface-descriptor.md)> | no (yes) | Available access interfaces of the service instance. Mandatory if the _bridgeId_ is not specified in the containing model.