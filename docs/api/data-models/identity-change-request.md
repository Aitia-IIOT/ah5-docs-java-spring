# IdentityChangeRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
systemName | [SystemName](../primitives.md#systemname) | yes | The requester of the operation.
credentials | [Credentials](../data-models/credentials.md) | yes | Credential information related to the system.
newCredentials | [Credentials](../data-models/credentials.md) | yes | The new credential information that replaces the current one.
