# AuthorizationTokenGenerationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
tokenVariant | [AccessTokenVariant](../primitives.md#accesstokenvariant) | yes | Exact token technology.
provider | [SystemName](../primitives.md#systemname) | yes | Name of the targeted provider system.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | no | Type of the targeted resource. Default is `SERVICE_DEF`.
target | [ServiceName](../primitives.md#servicename) or [EventTypeName](../primitives.md#eventtypename) | yes | Target of the token.
scope | [ServiceOperationName](../primitives.md#serviceoperationname) | no | Scope of the token. Only matters when the target is a service definition.
