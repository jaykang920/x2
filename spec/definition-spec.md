x2 Definition Specification v1.0
================================

Definition Unit
---------------

The x2 definition of an application domain is x-piled (trans-piled or
cross-piled) by each unit, which is normally a single text file. An x2
definition unit has the following attributes.

### namespace

Specifies the source package path where the x-piled source file resides. If this
optional attribute is omitted, then the resultant source file would belong to
the global (unnamed or default) package of the target language.

Each sub-path of the composite package path should be separated by a forward
slash ('/') as a package path separator, regardless of the target language.

Reference Elements
------------------

An x2 definition unit may contain one or more of the following reference
elements.

### namespace

Specifies a namespace reference. Namespace references are used by the languages
that require package reference (such as C# and Java).

| Attribute         | Description                |
|-------------------|----------------------------|
| target (required) | Target source package path |

### file

Specifies a definition file reference. File references are used by the languages
that require header file includes (such as C/C++).

| Attribute         | Description                                 |
|-------------------|---------------------------------------------|
| target (required) | Relative path to the target definition unit |

Definition Elements
-------------------

An x2 definition unit may have one or more of the following definition elements.

### consts

Defines a set of shared constant name/value pairs.

| Attribute       | Description                                    |
|-----------------|------------------------------------------------|
| name (required) | Name of the constant set                       |
| type (optional) | Type of the child constants (int32 by default) |

The value of the **type** attribute of a **consts** element is limited to the
built-in primitive data types.
A **consts** element may contain one or more of the following child elements.

* **const**

Defines a constant value.

| Attribute        | Description           |
|------------------|-----------------------|
| name (required)  | Name of the constant  |
| value (required) | Value of the constant |

### cell

Defines an x2 cell.

| Attribute       | Description                               |
|-----------------|-------------------------------------------|
| name (required) | Name of the cell                          |
| base (optional) | Parent cell which this cell inherits from |

A **cell** element may contain one or more of the following child elements.

* **property**

Defines a cell property.

| Attribute                | Description                                |
|--------------------------|--------------------------------------------|
| name (required)          | Name of the property                       |
| type (required)          | Data type of the property                  |
| default value (optional) | Default value of the property, if required |

A property element may have a specified default value.

The order of property element definitions matters. Once published a cell
definition, you should not change the property definition order unless you give
up the backwards-compatibility.

### event

Defines an x2 event.

| Attribute       | Description                               |
|-----------------|-------------------------------------------|
| name (required) | Name of the cell                          |
| id (required)   | Event type identifier that is unique within the application domain |
| base (optional) | Parent cell which this cell inherits from |

An **event** element may contain one or more of the child elements of a **cell**.

Built-In Data Types
-------------------

A property data type may be either one of the following built-in types or
another cell/event type.

### Primitive Data Types

| Name     | Description                                     | Default Value |
|----------|-------------------------------------------------|---------------|
| bool     | Boolean (true/false) value                      | false         |
| byte     | 1-byte (8-bit) unsigned integer                 | 0             |
| int8     | 1-byte (8-bit) signed integer                   | 0             |
| int16    | 2-byte (16-bit) signed integer                  | 0             |
| int32    | 4-byte (32-bit) signed integer                  | 0             |
| int64    | 8-byte (64-bit) signed integer                  | 0             |
| float32  | Single-precision (4-byte) floating point number | 0.0           |
| float64  | Double-precision (8-byte) floating point number | 0.0           |
| string   | Character string                                | ""            |
| datetime | Represents an instant in time, typically expressed as a date and time of day in UTC (Coordinated Universal Time) | undefined |

Please note that the x2 definition does not support unsigned integer types
except byte, since some platforms or languages do not support unsigned integer
types.

### Composite Data Types

| Name      | Description                      | Default Value |
|-----------|----------------------------------|---------------|
| bytes     | Fixed-length array of raw bytes  | null          |
| list(T)   | Ordered list of values           | null          |
| map(K, V) | Unordered map of key-value pairs | null          |

Definition Format
-----------------

Instead of introducing another IDL(Interface Definition Language), x2 adopts
existing text formats for definitions. Though YAML is less verbose, our first
choice is XML since it is supported out-of-box in most modern languages.

### XML Definition Format

The XML definition format does not have a DTD schema yet.

Follows a skeletal structure example of well-formed XML definition file.

```xml
<?xml version="1.0" encoding="utf-8"?>
<x2 namespace="">
  <references>
    <namespace target=""/>
    <file target=""/>
  </references>
  <definitions>
    <consts name="" type="">
      <const name="">value</const>
      ...
    </consts>
    <cell name="" base="">
      <property name="" type=""/>
      <property name="" type="">default value</property>
      ...
    </cell>
    <event name="" id="" base="">
      <property name="" type=""/>
      <property name="" type="">default value</property>
      ...
    </event>
  </definitions>
</x2>
```

### YAML Definition Format

Follows a skeletal structure example of YAML definition file.

```yaml
---
namespace:
references:
  - { type: namespace, target: }
  - { type: file, target: }
definitions:
  - { class: consts, name: , type: ,
      elements: [
        { name: , value: },
        { name: , value: },
      ]
    }
  - { class: cell, name: , base: ,
      properties: [
        { name: , type: },
        { name: , type: , default: },
      ]
    }
  - { class: event, name: , id: , base: ,
      properties: [
        { name: , type: },
        { name: , type: , default: },
      ]
    }
```
