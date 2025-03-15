# InterfaceTemplateQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pageNumber | [Number](../primitives.md#number) | no (yes) | The number of the requested page. It is mandatory, if page size is specified.
pageSize | [Number](../primitives.md#number) | no (yes) | The number of entries on the requested page. It is mandatory, if page number is specified.
pageSortField | [String](../primitives.md#string) | no | The identifier of the field which must be used to sort the entries.
pageDirection | [Direction](../primitives.md#direction) | no | The direction of the sorting.
templateNames | List<[InterfaceTemplate](../primitives.md#interfacetemplate)> | no | Requester is looking for interface templates with any of the specified names.
protocols | List<[Protocol](../primitives.md#protocol)> | no | Requester is looking for interface templates with any of the specified protocols.