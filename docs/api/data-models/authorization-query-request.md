# AuthorizationQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Page-related parameters.
level | [AuthorizationLevel](../primitives.md#authorizationlevel) | yes | Requester is looking for policy instances with the specified level (management-level or provider-level).
providers | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for policy instances that belong to any of the specified providers.
instanceIds | List<[AuthorizationPolicyInstanceID](../primitives.md#authorizationpolicyinstanceid)> | no | Requester is looking for policy instances with any of the specified identifiers. 
cloudIdentifiers | List<[CloudIdentifier](../primitives.md#cloudidentifier)> | no | Requester is looking for policy instances that belong to any of the specified clouds.
targetNames | List<[ServiceName](../primitives.md#servicename)> or List<[EventTypeName](../primitives.md#eventtypename)> | no | Requester is looking for policy instances that belong to any of the specified targets (either service definitions or event types).
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | no (yes) | The type of the specified targets. Mandatory if _targetNames_ are specified.

