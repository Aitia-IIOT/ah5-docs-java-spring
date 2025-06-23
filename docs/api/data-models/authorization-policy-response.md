# AuthorizationPolicyResponse

Field | Type | Description
--- | --- | --- 
instanceId | [AuthorizationPolicyInstanceID](../primitives.md#authorizationpolicyinstanceid) | Unique identifier of the policy instance.
authorizationLevel | [AuthorizationLevel](../primitives.md#authorizationlevel) | Level (provider or management) of the policy.
cloud | [CloudIdentifier](../primitives.md#cloudidentifier) | The cloud of the potential consumers. In case of the Local Cloud the word LOCAL is used.
provider | [SystemName](../primitives.md#systemname) | The name of the system who provides the target of the rule.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | The type of the target (service definition or event type).
target | [ServiceName](../primitives.md#servicename) or [EventTypeName](../primitives.md#eventtypename) | The type of the target (service definition or event type).
description | [String](../primitives.md#string) | The description of the rule.
defaultPolicy | [AuthorizationPolicyDescriptor](../data-models/authorization-policy-descriptor.md) | The policy details of the rule which is used when no more specialized policy details are available.
scopedPolicies | [ScopedPoliciesDescriptor](../data-models/scoped-policies-descriptor.md) | A structure that can contain specialized policy details.
createdBy | [SystemName](../primitives.md#systemname) | Authorization policy instance was created by this system.
createdAt | [DateTime](../primitives.md#datetime) | Authorization policy was registered at this timestamp.

