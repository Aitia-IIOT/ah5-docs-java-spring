# ServiceOrchestrationSubscriptionQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried subscription records.
ownerSystems | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for subscriptions with any of the specified creator system names.
targetSystems | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for subscriptions with any of the specified target consumer system names.
serviceDefinitions | List<[ServiceName](../primitives.md#servicename)> | no | Requester is looking for subscriptions with any of the specified service definition names.