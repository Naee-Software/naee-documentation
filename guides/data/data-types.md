# Data Types

Naee databases support several kind of data types:
### Simple types
 
| type | note  |
|:-|:-|
| Text  | simple text  |
| Encrypted text | when saved to the backend the data will no longer be visible but suitable for searches|
| Email| email formatted text |
| URL | URI formatted text |
| Number| decimal or integer numbers |
| Timestamp | date and time (as unix timestamp integer) |
| Boolean | true/false |

### Structured types

| Type | Note |
|:-|:-|
| Location |  a geo-referenced location |
| File | File document (see [Files](../storage/files.md)) |
| Object | an arbitrary structured dictionary[^] |
| Relation  | reference to another document (see [Relations](relations.md)) |
| List | array of any data type |
| List of relations | a list of related documents |

[^also known as map in some languages]