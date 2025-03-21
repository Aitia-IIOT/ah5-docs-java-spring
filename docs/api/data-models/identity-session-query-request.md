# IdentitySessionQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Page-related parameters.
namePart | [String](../primitives.md#string) | no | Requester is looking for active sessions of systems with names containing the specified text.
loginFrom | [DateTime](../primitives.md#datetime) | no | Requester is looking for active sessions that were created after the specified time.
loginTo | [DateTime](../primitives.md#datetime) | no | Requester is looking for active sessions that were created before the specified time.

