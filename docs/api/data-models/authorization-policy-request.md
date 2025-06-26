# AuthorizationPolicyRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
policyType | [AuthorizationPolicyType](../primitives.md#authorizationpolicytype) | yes | The type of the policy.
policyList | List<[SystemName](../primitives.md#systemname)> | no (yes) | A list of consumer system names. Mandatory in case of list-based policy type.
policyMetadataRequirement | [MetadataRequirements](../data-models/metadata-requirements.md)| no (yes) | System-level metadata requirements. Mandatory in case of metadata-based policy type.
