# DeviceQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pageNumber | [Number](../primitives.md#number) | no (yes) | The number of the requested page. It is mandatory if page size is specified.
pageSize | [Number](../primitives.md#number) | no (yes) | The number of entries on the requested page. It is mandatory if page number is specified.
pageSortField | [String](../primitives.md#string) | no | The identifier of the field which must be used to sort the entries.
pageDirection | [Direction](../primitives.md#direction) | no | The direction of the sorting.
deviceNames | List<[Name](../primitives.md#name)> | no | Requester is looking for devices with any of the specified names.
addresses | List<[Address](../primitives.md#address)> | no | Requester is looking for devices with any of the specified addresses.
addressType | [AddressType](../primitives.md#addresstype) | no | Requester is looking for devices with the specified type of address.
metadataRequirementsList | List<[MetadataRequirements](../data-models/metadata-requirements.md)> | no | Requester is looking for devices that are matching any of the specified metadata requirements.