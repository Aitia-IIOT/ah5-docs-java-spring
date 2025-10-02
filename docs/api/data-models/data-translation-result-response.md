# DataModelTranslationResultResponse

Field | Type | Description
--- | --- | ---
status | [DataModelTranslationTaskStatus](../primitives.md#datamodeltranslationtaskstatus) | Status of the translation task.
result | [String](../primitives.md#string) | Base64 encoded byte array (UTF-8 charset) of the output (translated) data payload or an error message.
mimeType | [MimeType](../primitives.md#mimetype) | The MIME type of the result data payload.