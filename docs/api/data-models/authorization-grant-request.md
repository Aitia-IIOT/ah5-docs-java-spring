# AuthorizationGrantRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
cloud | [CloudIdentifier](../primitives.md#cloudidentifier) | no | The cloud of the potential consumers. Omitted in case of the Local Cloud.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | yes | The type of the target (service definition or event type).
target | [ServiceName](../primitives.md#servicename) or [EventTypeName](../primitives.md#eventtypename) | yes | The target of the rule.
description | [String](../primitives.md#string) | no | The description of the rule.
defaultPolicy | [AuthorizationPolicyRequest](../data-models/authorization-policy-request.md) | yes | The policy details of the rule which is used when no more specialized policy details are available.
scopedPolicies | [ScopedPoliciesRequest](../data-models/scoped-policies-request.md) | no | A structure that can contain specialized policy details.