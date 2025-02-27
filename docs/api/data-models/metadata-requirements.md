# MetadataRequirements

A special Object which maps String keys to Object, primitive or list values, where

- Keys can be paths (or multi-level keys) which access a specific value in a Metadata structure, where parts
of the path are delimited with dot character (e.g. in case of ”key.subkey” path we are looking for the key
named ”key” in the metadata, which is associated with an embedded object and in this object we are
looking for the key named ”subkey”).

- Values are special Objects with two fields: an "operation" (e.g. less than) and an actual value (e.g. a
number). A metadata is matching a requirement if the specified operation returns true using the metadata
value referenced by a key path as first and the actual value as second operands.

- Alternatively, values can be ordinary primitives, lists or Objects. In this case the "operation" is equals by
default.

## Operations

**all kind of values (any) - all kind of values (any)**

`EQUALS`, `NOT_EQUALS`

**text-text**

`EQUALS_IGNORE_CASE`, `NOT_EQUALS_IGNORE_CASE`, `INCLUDES`,
`NOT_INCLUDES`, `INCLUDES_IGNORE_CASE`, `NOT_INCLUDES_IGNORE_CASE`,
`STARTS_WITH`, `NOT_STARTS_WITH`, `STARTS_WITH_IGNORE_CASE`, `NOT_STARTS_WITH_IGNORE_CASE`,
`ENDS_WITH`, `NOT_ENDS_WITH`, `ENDS_WITH_IGNORE_CASE`, `NOT_ENDS_WITH_IGNORE_CASE`, `REGEXP`

**number-number**

`LESS_THAN`, `LESS_THAN_OR_EQUALS_TO`, `GREATER_THAN`, `GREATER_THAN_OR_EQUALS_TO`

**text or list - number**

`SIZE_EQUALS`, `SIZE_NOT_EQUALS`

**list - any**

`CONTAINS`, `NOT_CONTAINS`

**any - list**

`IN`, `NOT_IN`
