# ServiceOrchestrationSubscriptionRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
orchestrationRequest | [ServiceOrchestrationRequest](../data-models/service-orchestration-request.md) | yes | Orchestration request details.
notifyInterface | [ServiceOrchestrationNotifyInterface](../data-models/service-orchestration-notify-interface.md) | yes | Interface details for sending push notifications.
duration | [Number](../primitives.md#number) | no | The interval while the subscription is active.