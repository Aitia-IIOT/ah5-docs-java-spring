# ServiceOrchestrationLockQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried lock records.
ids | List<[Number](../primitives.md#number)> | no | Requester is looking for lock records with any of the specified record identifiers.
orchestrationJobIds | List<[UUID](../primitives.md#uuid)> | no | Requester is looking for lock records with any of the specified orchestration job identifiers.
serviceInstanceIds | List<[ServiceInstanceID](../primitives.md#serviceinstanceid)> | no | Requester is looking for lock records with any of the specified service instance identifiers.
owner | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for lock records with any of the specified owner system names.
expiresBefore | [DateTime](../primitives.md#datetime) | no |  Requester is looking for lock records that expire before this timestamp.
expiresAfter | [DateTime](../primitives.md#datetime) | no |  Requester is looking for lock records that expire after this timestamp.