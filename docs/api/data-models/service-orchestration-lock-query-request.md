# ServiceOrchestrationLockQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried lock records.
ids | List<[Number](../primitives.md#number)> | no | Requester is looking for lock records with any of the specified record identifiers.
orchestrationJobIds | List<[UUID](../primitives.md#uuid)> | no | Requester is looking for lock records with any of the specified orchestration job identifiers.
serviceInstanceIds | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for lock records with any of the specified service instance identifier.
owner | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for lock records with any of the specified owner system names.
expiresBefore | [DateTime](../primitives.md#datetime) | no |  Requester is looking for lock records that expires before this timestamp.
expiresAfter | [DateTime](../primitives.md#datetime) | no |  Requester is looking for lock records that expires after this timestamp.