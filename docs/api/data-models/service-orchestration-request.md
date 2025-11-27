# ServiceOrchestrationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
serviceRequirement | [ServiceOrchestrationRequiremet](../data-models/service-orchestration-requirement.md) | yes | Details of the targeted service.
orchestrationFlags | List<[ServiceOrchestrationFlag](../data-models/service-orchestration-flag.md)> | no | List of orchestration flags to control the orchestration process.
qualityRequirements | List<[QoSRequirement](../data-models/qos-requirement.md)> | no | Service quality requirements in order of priority.
exclusivityDuration | [Number](../primitives.md#number) | no | The interval the service wanted to be exclusive.