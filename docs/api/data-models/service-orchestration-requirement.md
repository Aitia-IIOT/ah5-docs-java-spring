# ServiceOrchestrationRequirement

Field | Type | Mandatory | Description
--- | --- | --- | ---
serviceDefinition | [ServiceName](../primitives.md#servicename) | yes/no | The required service definition name. Mandatory in case of **dynamic** strategy.
operations | List<[ServiceOperationName](../primitives.md#serviceoperationname)> | yes/no | The required service operation names. Exactly one operation must be defined, when the following orchestration flags are true: `ONLY_INTERCLOUD`, `ALLOW_INTERCLOUD`, `ALLOW_TRANSLATION`
versions | List<[Version](../primitives.md#version)> | no | The required service versions.
alivesAt | [DateTime](../primitives.md#datetime) | no | The orchestrated service must be alive by this time.
metadataRequirements | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | The orchestrated service must meet at least one of the specified metadata requirement.
interfaceTemplateNames | List<[InterfaceName](../primitives.md#interfacename)> | no | The orchestrated service must offer at least one from the specified interface template names.
interfaceAddressTypes | List<[AddressType](../primitives.md#addresstype)> | no | The orchestrated service must offer at least one from the specified interface address types.
interfacePropertyRequirements | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | The orchestrated service must offer at least one interface that meets with one of the specified property requirements.
securityPolicies | List<[SecurityPolicy](../primitives.md#securitypolicy)> | no | The orchestrated service must meet one of the specified security policies.
preferredProviders | List<[SystemName](../primitives.md#systemname)> | no | Provider system names specified here have priority.