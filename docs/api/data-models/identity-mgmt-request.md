# IdentityMgmtRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
systemName | [SystemName](../primitives.md#systemname) | yes | Unique identifier of the identifiable system.
credentials | [Credentials](../data-models/credentials.md) | yes | Authentication method-specific credential information of the system.
sysop | [Boolean](../primitives.md#boolean) | no | It determines whether the identifiable system has higher level administration rights or not.
