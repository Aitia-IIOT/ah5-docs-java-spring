# ServiceOrchestrationHistoryQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried lock records.
ids | List<[UUID](../primitives.md#uuid)> | no | Requester is looking for job records with any of the specified job identifiers.
statuses | List<[ServiceOrchestrationJobStatus](../primitives.md#serviceorchestrationjobstatus)> | no | Requester is looking for job records with any of the specified job statuses.
type | [ServiceOrchestrationType](../primitives.md#serviceorchestrationtype) | no | Requester is looking for job records with the specified type.
requesterSystems | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for job records with any of the specified requester systems.
targetSystems | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for job records with any of the specified target systems.
serviceDefinitions | List<[ServiceName](../primitives.md#servicename)> | no | Requester is looking for job records with any of the specified service definitions.
subscriptionIds | List<[UUID](../primitives.md#uuid)> | no | Requester is looking for job records with any of the specified subscription identifiers.
