x2
==

x2 is a set of concepts and specifications that facilitates the development of
highly flexible cross-platform, cross-language distributed systems. Based on a
few old simple yet powerful constructs, it encourages a unified loosely coupled
event-driven approach throughout an application of any scale.

*In a nutshell, x2 is all about how you organize your distributed application.*

Although x2 also specifies its own proprietary binary format and most ports come
with their default protocol code generators, x2 is differentiated from common
protocol code generators (such as Google ProtoBuf or Apache Thrift) in that it
provides a macroscopic model to flexibly organize your event-based distributed
system.

Concepts
--------

The conceptual constructs of x2 are defined in [concepts.md](concepts.md).

Specifications
--------------

The following documents provides the specifications that x2 ports should confirm:

* [definition.md](specs/definition.md) : x2 definition specifications
* [binary-format.md](specs/binary-format.md) : x2 proprietary binary format specifications

Ports
-----

Since x2 itself is not a single framework or library, we need its specific
embodiment in order to make use of it. A _port_ is an actual implementation of x2,
targeting a specific platform or language. A port is usually named in the form of
x2[target], where target signifies its platform or langauge. Especially when a
port is targeting a single programming language, it is normally named as
x2[source file extension]: e.g., x2py, x2rb, etc.

Follows the currently available ports of x2.

* [x2clr](https://github.com/jaykang920/x2clr) : CLR (.NET/Mono) port of x2,
  written in C# (**reference port**)

Early-stage ports under development:

* [x2boost](https://github.com/jaykang920/x2boost) : C++ port of x2, based on C++98
and [Boost](http://www.boost.org) libraries
