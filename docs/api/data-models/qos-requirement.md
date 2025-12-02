# QoSRequirement

Field | Type | Mandatory | Description
--- | --- | --- | ---
type | [QoSEvaluationType](../primitives.md#qosevaluationtype) | yes | Specifies an exact evaluation method.
operation | [QoSOperation](../primitives.md#qosoperation) | yes | Specifies the operation to handle the QoS evaluation results.
requirements | [Metadata](../data-models/metadata.md) | no | Specifies the QoS evaluation method specific properties.