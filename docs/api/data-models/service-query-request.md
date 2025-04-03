# ServiceQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried service instances.
instanceIds | List<[ServiceInstanceID](../primitives.md#serviceinstanceid)> | no (yes) | Requester is looking for service instances with any of the specified identifiers. Mandatory if no providerNames nor serviceDefinitionNames are specified.
providerNames | List<[Name](../primitives.md#name)> | no (yes) | Requester is looking for service instances that are provided by any of the specified systems. Mandatory if no serviceInstanceIds nor serviceDefinitionNames are specified.
serviceDefinitionNames | List<[Name](../primitives.md#name)> | no (yes) | Requester is looking for service instances with any of the specified service definition names. Mandatory if no serviceInstanceIds nor providerNames are specified.
versions | List<[Version](../primitives.md#version)> | no | Requester is looking for service instances with any of the specified versions.
alivesAt | [DateTime](../primitives.md#datetime) | no | Requester is looking for service instances that will be available at the specified moment of the future.
metadataRequirementsList | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | Requester is looking for service instances that are matching any of the specified metadata requirements.
addressTypes | List<[AddressType](../primitives.md#addresstype)> | no | Requester is looking for service instances with interfaces whose access addresses are matching any of these types.
interfaceTemplateNames | List<[Name](../primitives.md#name)> | no | Requester is looking for service instances with any of the specified interface template names.
interfacePropertyRequirementsList | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | Requester is looking for service instances with interfaces that are matching any of the specified properties requirements.
policies | List<[SecurityPolicy](../primitives.md#securitypolicy)> | no | Requester is looking for service instances with any of the specified security policies.