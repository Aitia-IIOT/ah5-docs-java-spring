# LogRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Page-related parameters.
from | [DateTime](../primitives.md#datetime) | no | Requester is looking for log entries with timestamp that does not precede this one.
to | [DateTime](../primitives.md#datetime) | no | Requester is looking for log entries with timestamp that does not succeed this one.
severity | [LogSeverity](../primitives.md#logseverity) | no | Requester is looking for log entries with severity that equals to or is higher than the specified one.
loggerStr | [String](../primitives.md#string) | no | Requester is looking for log entries with logger whose name contains the specified text.
