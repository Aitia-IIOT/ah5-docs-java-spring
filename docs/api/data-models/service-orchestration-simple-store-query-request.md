# ServiceOrchestrationSimpleStoreQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried records.
ids | List<[UUID](../primitives.md#uuid)> | no |  Requester is looking for store entries with any of the specified identifiers.
consumerNames | List<[SystemName](../primitives.md#systemname)> | no (yes) | Requester is looking for store entries with any of the consumer system names.
serviceDefinitions | List<[ServiceName](../primitives.md#servicename)> | no (yes) | Requester is looking for store entries with any of the specified service definition names.
serviceInstanceIds | List<[ServiceInstanceId](../primitives.md#serviceinstanceid)> | no (yes) | Requester is looking for store entries with any of the specified service instance identifiers.
minPriority | [Number](../primitives.md#number) | no | Requester is looking for store entries with at least the specified priority.
maxPriority | [Number](../primitives.md#number) | no | Requester is looking for store entries with at most the specified priority.
createdBy | [SystemName](../primitives.md#systemname) | no (yes) | Requester is looking for store entries that were created by a system with the specified name.

**_Note:_** One of the following fields must be specified: `ids`, `consumerNames`, `serviceDefinitions`, `serviceInstanceIds` or `createdBy`