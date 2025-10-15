# TranslationDiscoveryServiceInstanceDescriptor

Field | Type | Mandatory | Description
--- | --- | --- | ---
instanceId | [ServiceInstanceID](../primitives.md#serviceinstanceid) | yes | Unique identifier of the service instance.
provider | [TranslationDiscoverySystemDescriptor](../data-models/translation-discovery-system-descriptor.md) | yes | Provider system.
serviceDefinition | [TranslationDiscoveryServiceDefinitionDescriptor](../data-models/translation-discovery-service-definition-descriptor.md) | yes | Service definition.
interfaces | List<[TranslationDiscovery- ServiceInstanceInterfaceDescriptor](../data-models/translation-discovery-service-instance-interface-descriptor.md)> | yes | Available access interfaces of the service instance.