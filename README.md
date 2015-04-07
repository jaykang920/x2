x2
==

_Under development now_

x2 is a set of concepts and specifications
(see [x2wiki](https://github.com/jaykang920/x2/wiki) for details) that facilitates
the development of highly flexible cross-platform, cross-language distributed
systems. Based on a few simple yet powerful constructs, it encourages a unified
loosely coupled event-driven approach throughout any scale of application.

Ports
-----

Since x2 itself is not a single framework or library, we need its specific
embodiment in order to make use of it. A _port_ is an actual implementation of x2,
targeting a specific platform or language. A port is usually named in the form of
x2[target], where target signifies its platform or langauge. Especially when a
port is targeting a single programming language, it is normally named as
x2[source file extension]: e.g., x2java, x2py, x2rb, etc.

Follows the currently available ports of x2.

* [x2clr](https://github.com/jaykang920/x2clr) : CLR (.NET/Mono) port of x2,
  written in C# (**reference port**)

Early-stage ports under development:

* [x2boost](https://github.com/jaykang920/x2boost) : C++ port of x2, based on C++98
and [Boost](http://www.boost.org) libraries
