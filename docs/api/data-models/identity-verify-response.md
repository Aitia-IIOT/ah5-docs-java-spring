# IdentityVerifyResponse

Field | Type | Description
--- | --- | --- 
verified | [Boolean](../primitives.md#boolean) | The result of the verification.
systemName | [Name](../primitives.md#name) | The name of the verified system. Empty if the verification was unsuccessful.
sysop | [Boolean](../primitives.md#boolean) | A flag that determines whether the verified system has higher level administrative rights. Empty the if verification was unsuccessful.
loginTime | [DateTime](../primitives.md#datetime) | The system started its active session at this time. Empty if the verification was unsuccessful.
expirationTime | [DateTime](../primitives.md#datetime) | The verified token is valid until this time. Empty if the verification was unsuccessful.
