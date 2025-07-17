# ServiceOrchestrationLockResponse

Field | Type | Description
--- | --- | ---
id | [Number](../primitives.md#number) | Lock record unique ID.
orchestrationJobId | [UUID](../primitives.md#uuid) | ID of the associated orchestration job if any.
serviceInstanceId | [ServiceInstanceID](../primitives.md#serviceinstanceid) | ID of the locked service instance.
owner | [SystemName](../primitives.md#systemname) | The system the lock belongs to.
expiresAt | [DateTime](../primitives.md#datetime) | The lock is active until this timestamp.
temporary | [Boolean](../primitives.md#boolean) | If true, the lock was made during an orchestration process and possibly will be released automatically within a short time (if service instance is not chosen to be reserved).
