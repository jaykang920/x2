x2 Implementation Specification
===============================

Events
------

### Required Internal Properties

For compatibility reason, every event implementation should include the following internal properties.

Note that the internal property names start with underscore(_) to avoid conflict
with user-defined property names.

| Fingerprint Index | Name        | Type  | Serialized |
|-------------------|-------------|-------|------------|
| 0                 | _Handle     | int32 | No         |
| 1                 | _WaitHandle | int32 | Yes        |

#### _Handle

THis property helps track the intra-process request-response pairing.
It is excluded on (de)serialization.

#### _WaitHandle

This property helps track the inter-process request-response pairing.
