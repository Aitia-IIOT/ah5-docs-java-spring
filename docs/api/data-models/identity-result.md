# IdentityVerifyResponse

Field | Type | Description
--- | --- | --- 
systemName | [Name](../primitives.md#name) | Unique identifier of the identified system.
authenticationMethod | [AuthenticationMethod](../primitives.md#authenticationmethod) | The authentication method the identity uses.
sysop | [Boolean](../primitives.md#boolean) | It determines whether the identified system has higher level administration rights or not.
createdBy | [Name](../primitives.md#name) | The identity was created by this identified system.
createdAt | [DateTime](../primitives.md#datetime) | Identity was created at this timestamp.
updatedBy | [Name](../primitives.md#name) | The identity was modified by this identified system.
updatedAt | [DateTime](../primitives.md#datetime) | Identity was modified at this timestamp.
