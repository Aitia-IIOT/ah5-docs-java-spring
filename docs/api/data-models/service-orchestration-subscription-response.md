# ServiceOrchestrationSubscriptionResponse

Field | Type | Description
--- | --- | ---
id | [UUID](../primitives.md#uuid) | Unique identifier of the subscription record.
ownerSystemName | [SystemName](../primitives.md#systemname) | Unique identifier of the creator system.
targetSystemName | [SystemName](../primitives.md#systemname) | Unique identifier of the subscribed system.
orchestrationForm | [ServiceOrchestartionRequest](../data-models/service-orchestration-request.md) | Orchestration request details.
notifyInterface | [ServiceOrchestrationNotifyInterface](../data-models/service-orchestration-notify-interface.md) | Interface details for sending push notifications.
expiredAt | [DateTime](../primitives.md#datetime) | The subscription is active until this time.
createdAt | [DateTime](../primitives.md#datetime) | The subscription was created at this timestamp.