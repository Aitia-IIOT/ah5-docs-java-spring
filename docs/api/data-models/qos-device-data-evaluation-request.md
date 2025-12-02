# QoSDeviceDataEvaluationConfig

Field | Type | Mandatory | Description
--- | --- | --- | ---
metricNames | List<[BasicDeviceKpiMetric](../primitives.md#basicdevicekpimetric)> | yes | Name of metrics to be considered for calculating the QoS score.
metricWeights | List<[Double](../primitives.md#double)> | no | Weights belonged to the metric names. If defined,the number of elements must be the same as the `metricNames` and their sum shall be 1. If not, all metric considered as equal.
timeWindow | [Number](../primitives.md#number) | no | The interval (in seconds) of measurements to be considered when calculating the QoS score. The system default value is used if no value is defined.
threshold | [Double](../primitives.md#double) | yes/no | A borderline of QoS score above which a system is considered as non-compliant. Mandatory in case of `filter` operaiton and ignored in case of `sort` operation.