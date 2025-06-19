# AuthorizationLookupRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
instanceIds | List<[AuthorizationPolicyInstanceID](../primitives.md#authorizationpolicyinstanceid)> | no (yes) | Requester is looking for policy instances with any of the specified identifiers. Mandatory if no _cloudIdentifiers_ nor _targetNames_ are specified.
cloudIdentifiers | List<[CloudIdentifier](../primitives.md#cloudidentifier)> | no (yes) | Requester is looking for policy instances that belongs to any of the specified clouds. Mandatory if no _instanceIds_ nor _targetNames_ are specified.
targetNames | List<[ServiceName](../primitives.md#servicename)> or List<[EventTypeName](../primitives.md#eventtypename)> | no (yes) | Requester is looking for policy instances that belongs to any of the specified targets (either service definitions or event types). Mandatory if no _instanceIds_ nor _cloudIdentifiers_ are specified.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | no (yes) | The type of the specified targets. Mandatory if _targetNames_ are specified.
