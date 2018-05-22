Changelog
=========

v1.1
----

### Definition Specification

* added support for collection type map(K, V) which is an unordered map of key-
value pairs.

### Wire Format Specification

* Fixed the embedded cell/event serialization rule: partial serialization for
embedded cells, while full serialization for embedded events.

v1.0
----

### Definition Specification

* Added reference elements to handle namespace/file reference specifications.
* Modified XML definition format to introduce list-holding parent tags.
* Added secondary YAML definition format.
