# InterfaceTemplateQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried interface templates.
templateNames | List<[InterfaceName](../primitives.md#interfacename)> | no | Requester is looking for interface templates with any of the specified names.
protocols | List<[Protocol](../primitives.md#protocol)> | no | Requester is looking for interface templates with any of the specified protocols.