# IdentityQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Page-related parameters.
namePart | [String](../primitives.md#string) | no | Requester is looking for identities with system names containing the specified text.
isSysop | [Boolean](../primitives.md#boolean) | no | Requester is looking for identities that have/have not higher level administration rights depending of the specified value.
createdBy | [Name](../primitives.md#name) | no | Requester is looking for identities that have been created by the specified identity.
creationFrom | [DateTime](../primitives.md#datetime) | no | Requester is looking for identities that were created after the specified time.
creationTo | [DateTime](../primitives.md#datetime) | no | Requester is looking for identities that were created before the specified time.
hasSession | [Boolean](../primitives.md#boolean) | no | Requester is looking for identities that have/have not active session at the moment.

