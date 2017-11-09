x2 Wire Format Specification v1.0
=================================

Basic Encoding
--------------

### Variable-Length Integer Encoding

The variable-length encoding of 32/64-bit integer values relies on base-128
variants (unsigned LEB128), where each byte, except the last one, has the MSB
(most significant bit) set indicating that there are further bytes to come. The
lower 7 bits of each byte are used to store the two's complement representation
of the number in groups of 7 bits, least significant group first.

| 32-bit Integer Range    | Encoded Bytes (LSG First)                    |
|-------------------------|----------------------------------------------|
| 0x00000000 - 0x0000007f | 0xxxxxxx                                     |
| 0x00000080 - 0x00003fff | 1xxxxxxx 0xxxxxxx                            |
| 0x00004000 - 0x001fffff | 1xxxxxxx 1xxxxxxx 0xxxxxxx                   |
| 0x00200000 - 0x0fffffff | 1xxxxxxx 1xxxxxxx 1xxxxxxx 0xxxxxxx          |
| 0x10000000 - 0xffffffff | 1xxxxxxx 1xxxxxxx 1xxxxxxx 1xxxxxxx 0000xxxx |

For signed 32/64-bit integer values, instead of signed LEB128, we adopt Google
Protocol Buffers' more simple method (so-called ZigZag encoding), where they are
encoded with unsigned LEB128 along with the following manipulation:

```c
(n << 1) ^ (n >> 31)  // for 32 bits
(n << 1) ^ (n >> 63)  // for 64 bits
```

where the second shift (n >> 31/63) is an arithmetic shift so that the (n << 1)
part is negated for negative numbers.

### Fixed-Length Integer Encoding

The fixed-length encoding of integer values follows network (big-endian) byte
order.

Binary Format
-------------

### Encoding Built-In Types

Each built-in data types in x2 is encoded as follows.

| Type     | Encoding                                             |
|----------|------------------------------------------------------|
| bool     | 1 byte (0 for false, 1 for true)                     |
| byte     | unsigned byte                                        |
| int8     | 8-bit signed integer                                 |
| int16    | Fixed 16-bit signed integer                          |
| int32    | ZigZag encoded signed 32-bit integer                 |
| int64    | ZigZag encoded signed 64-bit integer                 |
| float32  | Fixed big-endian 32 bits (IEEE 754 single-precision) |
| float64  | Fixed big-endian 64 bits (IEEE 754 double-precision) |
| string   | 32-bit integer length in unsigned LEB128, followed by UTF-8 encoded bytes |
| datetime | Fixed 64-bit signed integer that represents the number of milliseconds elapsed since Unix epoch (00:00:00.000 on 1 January 1970) |
| bytes    | 32-bit integer count in unsigned LEB128, followed by the sequence of raw bytes |
| list(T)  | 32-bit integer count in unsigned LEB128, followed by the sequence of value type |

### Encoding User-Defined Types

x2 cells and events maintain their own fingerprints to keep track of which
property has been set explicitly. And the fingerprints should precede their
user-defined properties to determine which property is to be encoded/decoded.
An x2 fingerprint is encoded as follows.

| Field  | Description                                                        |
|--------|--------------------------------------------------------------------|
| Length | 32-bit integer fingerprint length in bits, unsigned LEB128 encoded |
| Bytes  | Sequence of fingerprint bytes, least significant byte first        |

An x2 cell is encoded as follows. Please note that a cell does not have any type
identifier. In order for a cell to be transferred across processes, it should be
contained in an x2 event.

| Field       | Description                                                  |
|-------------|--------------------------------------------------------------|
| Length      | 32-bit integer cell length in bytes, unsigned LEB128 encoded |
| Fingerprint | Fingerprint encoded                                          |
| Properties  | Ordered sequence of property values which are marked in the fingerprint |

For cells and events, A null reference is expressed by a length value zero(0).

#### Partial Serialization

When an extended (derived) cell/event instance is serialized in place of a base
(super) class, it should be serialized as if it were a base class instance.

Link-Specific Format
--------------------

### TCP Socket Link

In TCP socket links, an x2 event is transferred as follows.

| Field           | Description                                                |
|-----------------|------------------------------------------------------------|
| Header          | 32-bit unsigned integer header, unsigned LEB128 encoded    |
| Type Identifier | Signed 32-bit integer event type identifier, ZigZag encoded |
| Event           | Event encoded, except the preceding length                 |

#### Header Integer format

| Bits       | Description                                                     |
|------------|-----------------------------------------------------------------|
| bit31~bit1 | Payload length in bytes: positive 32-bit integer left-shifted by 1 bit |
| bit0 (LSB) | Boolean flag indicating whether the payload has been transformed or not |

```c
header = (length << 1) | (transformed ? 1: 0)
```
