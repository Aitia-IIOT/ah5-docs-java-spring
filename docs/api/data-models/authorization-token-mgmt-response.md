# AuthorizationTokenMgmtResponse

Field | Type | Description
--- | --- | ---
tokenType | [TokenType](../primitives.md#tokentype) | Type of token technology group.
variant | [AccessTokenVariant](../primitives.md/#accesstokenvariant) | Exact type of token technology.
token | [AccessToken](../primitives.md#accesstoken) | The token itself.
tokenReference | [String](../primitives.md#string) | Reference of the token record.
requester | [SystemName](../primitives.md#systemname) | Name of the system that requested the token.
consumerCloud | [CloudIdentifier](../primitives.md#cloudidentifier) | Cloud of the consumer.
consumer | [SystemName](../primitives.md#systemname) | Name of the consumer system.
provider | [SystemName](../primitives.md#systemname) | Name of the provider system.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | Type of the targeted resource.
target | [ServiceName](../primitives.md#servicename) or [EventTypeName](../primitives.md#eventtypename) | Target of the token.
scope | [ServiceOperationName](../primitives.md#serviceoperationname) | Scope of the token. Only matters when the target is a service definition. If null, the token is valid for all the service-operation.
createdAt | [DateTime](../primitives.md#datetime) | Token was generated at this timestamp.
usageLimit | [Number](../primitives.md#number) | Maximum number of token usage, if any.
usageLeft | [Number](../primitives.md#number) | The token can still be used this many times, if any.
expiresAt | [DateTime](../primitives.md#datetime) | Token is valid until this timestamp, if any.