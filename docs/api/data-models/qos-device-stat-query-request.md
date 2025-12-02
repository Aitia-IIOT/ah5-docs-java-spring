# QoSDeviceStatQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried records.
metricGroup | [BasicDeviceKpiMetricGroup](../primitives.md#basicdevicekpimetricgroup) | yes | Requester is looking for records with the specified metric group.
from | [DateTime](../primitives.md#datetime) | no | The requester is looking for records that are not older than the specified timestamp.
to | [DateTime](../primitives.md#datetime) | no | The requester is looking for records that are not newer than the specified timestamp.
aggregation | List<[BasicDeviceKpiMetricAggregation](../primitives.md#basicdevicekpimetricaggregation)> | no | Requester is looking for records with any of the specified aggregations.
systemNames | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for records associated with any of the specified systems.