# AuthorizationTokenGenerationMgmtRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
tokenVariant | [AccessTokenVariant](../primitives.md#accesstokenvariant) | yes | Exact token technology.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | no | Type of the targeted resource. Default is `SERVICE_DEF`.
consumerCloud | [CloudIdentifier](../primitives.md#cloudidentifier) | no | Cloud of the consumer. Default is `LOCAL`.
consumer | [SystemName](../primitives.md#systemname) | yes | Name of the consumer system.
provider | [SystemName](../primitives.md#systemname) | yes | Name of the targeted provider system.
target | [ServiceName](../primitives.md#servicename) or [EventTypeName](../primitives.md#eventtypename) | yes | Target of the token.
scope | [ServiceOperationName](../primitives.md#serviceoperationname) | no | Scope of the token. Only matters when the target is a service definition.
expiresAt | [DateTime](../primitives.md#datetime) | no | Token will be valid until this timestamp. Only in case of time limited tokens. Default time limit is applied if not defined.
usageLimit | [Number](../primitives.md#number) | no | How many times the token will be valid. Only in case of usage limited tokens. Default usage limit is applied if not defined.