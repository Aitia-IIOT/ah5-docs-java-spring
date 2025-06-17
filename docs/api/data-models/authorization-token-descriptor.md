# AuthorizationTokenDescriptor

Field | Type | Description
--- | --- | ---
tokenType | [TokenType](../primitives.md#tokentype) | Type of the token.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | Type of the target.
token | [String](../primitives.md#string) | The token itself.
usageLimit | [Number](../primitives.md#number) | Maximum number of token usage, if any.
expiresAt | [DateTime](../primitives.md#datetime) | Token is valid until this time, if any.