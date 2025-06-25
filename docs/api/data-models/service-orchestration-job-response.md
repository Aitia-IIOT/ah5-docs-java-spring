# ServiceOrchestrationJobResponse

Field | Type | Description
--- | --- | ---
id | [UUID](../primitives.md#uuid) | Unique job identifier.
status | [JobStatus](../primitives.md#serviceorchestrationjobstatus) | Actual working state of the job.
type | [ServiceOrchestrationType](../primitives.md#serviceorchestrationtype) | Type of orchestration.
requesterSystem | [SystemName](../primitives.md#systemname) | Name of the system that started the orchestration process.
targetSystem | [SystemName](../primitives.md#systemname) | Name of the system for which the orchestration is executed.
serviceDefinition | [ServiceName](../primitives.md#servicename) | Name of the service that the orchestration job is targeting.
subscriptionId | [UUID](../primitives.md#uuid) | Unique identifier of associated subscription record.
message | [String](../primitives.md#string) | Additional error or warning information.
createdAt | [DateTime](../primitives.md#datetime) | The job was created at this timestamp.
startedAt | [DateTime](../primitives.md#datetime) | The job was started at this timestamp.
finishedAt | [DateTime](../primitives.md#datetime) | The job was finished at this timestamp.