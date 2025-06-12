# BlacklistQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried blacklist entries.
systemNames | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for blacklist entries that apply to systems with any of the specified names.
mode | [Mode](../primitives.md#mode) | no | Requester is looking for blacklist entries with the specified activity.
issuers | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for blacklist entries that were created by systems with any of the specified names.
revokers | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for blacklist entries that were revoked by systems with any of the specified names.
reason | [Reason](../primitives.md#reason) | no | Requester is looking for blacklist entries that were created for the specified reason.
alivesAt | [DateTime](../primitives.md#datetime) | no | Requester is looking for active blacklist records that are not expired at this timestamp.