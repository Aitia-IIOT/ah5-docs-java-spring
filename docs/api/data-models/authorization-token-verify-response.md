# AuthorizationTokenVerifyResponse

Field | Type | Description
--- | --- | ---
verified | [Boolean](../primitives.md#boolean) | The result of the verification.
consumerCloud | [CloudIdentifier](../primitives.md#cloudidentifier) | The cloud of the consumer the token is associated with.
consumer | [SystemName](../primitives.md#systemname) | Name of the consumer the token is associated with.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | Type of the targeted resource the token is associated with.
target | [ServiceName](../primitives.md#servicename) or [EventTypeName](../primitives.md#eventtypename) | The target the token is associated with.
scope | [ServiceOperationName](../primitives.md#serviceoperationname) | Scope of the token. Only matters when the target is a service definition. If null, the token is valid for all the operations of the service definition target.
