# LogEntry

Field | Type | Description
--- | --- | --- 
logId | [String](../primitives.md#string) | Unique identifier of the entry.
entryDate | [DateTime](../primitives.md#datetime) | The timestamp of the entry.
logger | [String](../primitives.md#string) | The logger that created the entry.
severity | [LogSeverity](../primitives.md#logseverity) | The severity of the entry.
message | [String](../primitives.md#string) | The log message.
exception | [String](../primitives.md#string) | If the log entry is an error, information of the related exception may appear here.
