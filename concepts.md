x2 Concepts
===========

An x2 port is required to implement all of the core concepts described below. In
addition, it may include more concepts to help specific aspects of application
development.

Core Concepts
-------------

### Shared Knowledge

Distributed applications usually depend on certain knowledge shared among their
participating peers. This kind of shared knowledge includes common data
structure and communication message format.

The shared knowledge of an application, defined in definition files, is
*x-piled* (trans-piled or cross-piled) into corresponding source files to form a
part of the entire application.

#### Constant

Named *constant* values may be shared through x2 definitions. x2 event type
identifiers are generally defined as shared integer constants.

#### Cell

A *cell* is a language-neutrally predefined common data structure shared among
x2 processes that serves a single application domain. As a composite data
record, a cell contains one or more *properties* whose data types may be either
primitive or composite (another cell).

##### Cell Characteristics

###### Hierarchical

x2 cells may be organized in a hierarchical structure, which is expressed as
class inheritance hierarchy in object-oriented languages. You can extract
properties repeated across many cells into a common parent cell and let the
child cells extend it.

###### Self-descriptive

An x2 cell is expected to describe itself (name-value pairs of properties) with
a simple toString-like call. This minimizes the need for manual string formatting.

###### Fingerprinted

Every cell instance has its own fingerprint that reflects its property
assignment status.

#### Event

An *event* is just a special kind of cell that can travel across different
flows. In order for a remote recipient to identify the events it receives,
events carry their own type identifiers unique within a single application
domain. Basically being a cell, an event may contain another cells, thus another
events.

Exchanging events should be the only way that cases/flows communicate each other.

#### Fingerprint

A *fingerprint* is an ordered set of boolean values indicating whether each
cell/event property has been touched (explicitly assigned) or not. Precise event
dispatching and efficient wire encoding of x2 relies on the fingerprints that
keep track of the properties status.

### Structural Components

Structural components are used to organize an x2 application with a simple and
unified design approach.

#### Flow

A *flow* is an independent execution unit that consumes or produces events.
Flows need to be attached to the hub in order to be notified with application
events. Each flow has its own event queue (if required) and execution context,
but its implementation details are not regulated.

Please note that a flow is not necessarily a thread. Though we mostly use
single-threaded flows to keep things simple, some flows may run multiple threads
simultaneously. Some flows do not even have its own thread so that it can be
embedded into existing applications.

#### Hub

A *hub* is an event distribution bus to which flows are attached. In most cases,
an x2 process maintains only one single hub with it. Once an event is *posted*
up to the hub, every attached flow is notified with it.

While a flow performs precise dispatching, a hub performs blind distribution,
meaning that a posted event is pushed into all the attached flows regardless of
whether they subscribed for that specific event or not.

#### Link

A *link* is a special kind of flow (or case, more desirably), which provides
inter-process communication across local or remote x2 processes. Normally links
are divided into clients and servers according to whether they actively initiate
a session or passively wait for one.

Since a link effectively isolates the communication details from the application
logic, it plays the key role in building flexible distributed applications with
x2.

More Concepts
-------------

### Structural Components

#### Case

Though flows already provide separate execution units, they are still tightly
coupled with their physical threading models. And it gives us little benefit
when we need to re-organize the overall threading architecture. So we introduce
a *case*, a more logical execution unit, in order to support flexible
re-engineering of threading architecture. In this idea, a flow is considered as
a stack of relevant cases which are initialized and terminated along with their
holding flow.

#### Hub Case

Hub cases are used to organize application global setup/teardown tasks. They are
directly added to the hub and initialized/terminated along with the hub when it
starts up and shuts down.

#### Hub Channel

In some complicated applications, a lot of flows may be attached to a single
hub. When it is clear that some of the events are meaningful to only a limited
set of flows, blindly distributing those events through all the attached flows
is definitely not desirable. Ports may introduce *hub channel* to handle this
case. If a posted event is assigned to a specific channel, only the flows that
are explicitly subscribing to that channel would be notified with it.
