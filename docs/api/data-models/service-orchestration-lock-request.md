# ServiceOrchestrationLockRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
serviceInstanceId | [ServiceInstanceID](../primitives.md#serviceinstanceid) | yes | Service instance to be locked.
owner | [SystemName](../primitives.md#systemname) | yes | The system the lock belongs to.
expiresAt | [DateTime](../primitives.md#datetime) | yes | The lock will be active until this timestamp.