# BlacklisEntryResponse

Field | Type | Description
--- | --- | --- 
systemName | [SystemName](../primitives.md#systemname) | Unique identifier of the blacklisted system.
createdBy | [SystemName](../primitives.md#systemname) | Unique identifier of the system that created the record.
revokedBy | [SystemName](../primitives.md#systemname) | Unique identifier of the system that revoked the record. Only appears if the record was revoked.
createdAt | [DateTime](../primitives.md#datetime) | Blacklist record was created at this timestamp.
updatedAt | [DateTime](../primitives.md#datetime) | Blacklist record was updated at this timestamp.
reason | [BlacklistReason](../primitives.md#blacklistreason) | The system was blacklisted because of this reason.
expiresAt | [DateTime](../primitives.md#datetime) | Blacklist record expires at this timestamp. Only appears if the record can expire.
active | [Booelan](../primitives.md#boolean) | Indicates if the rule defined by the entry is active. Only false if the rule has been explicitly revoked.