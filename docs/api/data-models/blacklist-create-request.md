# BlacklistCreateRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
systemName | [SystemName](../primitives.md#systemname) | yes | The name of the system to be blacklisted.
expiresAt | [DateTime](../primitives.md#datetime) | no | This rule will expire at this timestamp.
reason | [Reason](../primitives.md#reason) | yes | The system is blacklisted because of this reason.