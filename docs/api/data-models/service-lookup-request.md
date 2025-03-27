# ServiceLookupRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
instanceIds | List<[ServiceInstanceID](../primitives.md#serviceinstanceid)> | no | Requester is looking for service instances with any of the specified identifiers. Mandatory if no providerNames nor serviceDefinitionNames are specified.
providerNames | List<[Name](../primitives.md#name)> | no | Requester is looking for service instances that are provided by any of the specified systems. Mandatory if no serviceInstanceIds nor serviceDefinitionNames are specified.
serviceDefinitionNames | List<[Name](../primitives.md#name)> | no | Requester is looking for service instances with any of the specified service definition names. Mandatory if no serviceInstanceIds nor providerNames are specified.
versions | List<[Version](../primitives.md#version)> | no | Requester is looking for service instances with any of the specified versions.
alivesAt | [DateTime](../primitives.md#datetime) | no | Requester is looking for service instances that will be available at the specified moment of the future.
metadataRequirementsList | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | Requester is looking for service instances that are matching any of the specified metadata requirements.
interfaceTemplateNames | List<[Name](../primitives.md#name)> | no | Requester is looking for service instances with any of the specified interface template names.
interfacePropertyRequirementsList | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | Requester is looking for service instances with interfaces that are matching any of the specified properties requirements.
policies | List<[SecurityPolicy](../primitives.md#securitypolicy)> | no | Requester is looking for service instances with any of the specified security policies.