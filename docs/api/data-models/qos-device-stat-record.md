# QoSDeviceStatRecord

Field | Type | Description
--- | --- | ---
metricGroup | [BasicDeviceKpiMetricGroup](../primitives.md#basicdevicekpimetricgroup) | The metric group of the result.
id | [Number](../primitives.md#number) | The numeric identifier of the record that is unique within the metric group.
timestamp | [DateTime](../primitives.md#datetime) | Timestamp of the record.
uuid | [UUID](../primitives.md#uuid) | The uuid of the associated device entity.
minimum | [Double](../primitives.md#double) | Statistical value.
mean | [Double](../primitives.md#double) | Statistical value. 
median | [Double](../primitives.md#double) | Statistical value.
maximum | [Double](../primitives.md#double) | Statistical value.
current | [Double](../primitives.md#double) | Statistical value.
systems | List<[SystemName](../primitives.md#systemname)> | Systems that the record is associated with.
