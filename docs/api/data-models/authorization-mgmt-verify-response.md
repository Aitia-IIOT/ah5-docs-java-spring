# AuthorizationMgmtVerifyResponse

Field | Type | Description
--- | --- | ---
provider | [SystemName](../primitives.md#systemname) | The name of the system that provides the target.
consumer | [SystemName](../primitives.md#systemname) | The name of the system that needs access to the target
cloud | [CloudIdentifier](../primitives.md#cloudidentifier) | The cloud of the consumer. In case of the Local Cloud the word _LOCAL_ can used.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | The type of the target (service definition or event type).
target | [ServiceName](../primitives.md#servicename) or [EventTypeName](../primitives.md#eventtypename) | The name of the target.
scope | [ServiceOperationName](../primitives.md#serviceoperationname) | The service operation that the consumer wants to use. Omitted, if it was not specified in the related request.
granted | [Boolean](../primitives.md#boolean) | The result of the verification.
