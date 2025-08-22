# AuthorizationPolicyDescriptor

Field | Type | Description
--- | --- | --- 
policyType | [AuthorizationPolicyType](../primitives.md#authorizationpolicytype) | The type of the policy.
policyList | List<[SystemName](../primitives.md#systemname)> | A list of consumer system names. Only filled in case of list-based policy type.
policyMetadataRequirement | [MetadataRequirements](../data-models/metadata-requirements.md)| System-level metadata requirements. Only filled in case of metadata-based policy type.
