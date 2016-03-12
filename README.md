x2
==

x2 is a set of concepts and specifications that facilitates the development of
highly flexible cross-platform, cross-language distributed systems. Based on a
few old simple yet powerful constructs, it encourages a unified loosely coupled
event-driven approach throughout an application of any scale.

*In a nutshell, x2 is all about how you organize your distributed application.*

Although x2 also specifies its own binary format and most ports come with their
default protocol code generators, x2 is differentiated from dedicated protocol
code generators (such as Google ProtoBuf or Apache Thrift) in that it provides a
macroscopic model to flexibly organize your event-based distributed
system.

Concepts
--------

The conceptual constructs of x2 are defined in [concepts.md](concepts.md).

Most of the concepts of x2 came from quite old tradition: flows are similar to
actors and events are exactly messages. There is nothing breaking new, but all
the concepts are tweaked to form a easily distributable architecture.

Fingerprint is just a trivial trick but it plays an important role in precise
event dispatching (with event equivalence) and efficient wire formatting.

Specifications
--------------

The following documents provides the specifications that x2 ports should confirm:

* [definition.md](specs/definition.md) : x2 definition specifications
* [binary-format.md](specs/binary-format.md) : x2 binary format specifications

x2 binary format is focused to reduce the payload size. It supports backwards-
compatibility if adding or removing properties happens only at the end of cells/
events preserving the order of the rest.

Though Behind
-------------

x2 is absolutely not the most efficient way to go in terms of runtime
performance. It is built on the modern belief that human efficiency should gain
more and more emphasis over machine efficiency.

The core philosophy of x2 is that it forces nothing upon developers but an
event-based architectural outline. That is why x2 is just a set of concepts and
specifications, instead of being a single complete product. You can use freely
any programming technique you want within your cases/flows. And you can even
write your own links to integrate legacy services.

Why x2
------

### Productivity

Set up a loosely-coupled event-driven architecture as your distributed
application backbone. In addition to generating protocol code that emits
efficient payload, it supports enhanced features such as cell/event inheritance
hierarchy and precise event dispatching, which enables programming-by-difference
and multi-property pattern matching respectively.

### Flexibility

Once you carefully organized events and partitioned your application logic, you
can easily change the deployment of your application in either inter-process
(distribution over network) or intra-process (threading model) scope.

### Testability

If your cases/flows stick to the rule that events are the only way they
communicate, then you can simply isolate a single case/flow for functional test
with a test case feeding the required events and investigating posted ones.

Ports
-----

Since x2 itself is not a concrete framework or library, we need its specific
implementation in order to make use of it. A *port* is an actual implementation
of x2, targeting a specific platform or language. A port is usually named in the
form of x2[target], where target signifies its platform or language. Especially
when a port is targeting a specific programming language, it is normally named
as x2[source file extension]: e.g., x2py, x2rb, etc.

Follows the currently available ports of x2.

* [x2clr](https://github.com/jaykang920/x2clr) : CLR (.NET/Mono) port of x2,
  written in C# (**reference port**)

Early-stage ports under development:

* [x2boost](https://github.com/jaykang920/x2boost) : C++ port of x2, based on C++98
and [Boost](http://www.boost.org) libraries
