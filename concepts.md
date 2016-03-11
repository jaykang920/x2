Concepts
========

An x2 port is required to implement all of the core concepts described below. In
addition, it may include additional common concepts to help specific aspects of
application development.

Core Concepts
-------------

#### Shared Knowledge

Distributed applications usually depend on certain knowledge shared among their
participating peers. This kind of shared knowledge includes common data
structure and communication message format.

The shared knowledge of an application, defined in definition files, is
*x-piled* (trans-piled or cross-piled) into corresponding source files to form a
part of the entire application.

#### Cell

A *cell* is a predefined common data structure shared among x2 processes that
serves a single application domain. As a composite data record, a cell contains
one or more *properties* whose data types may be either primitive or composite
(another cell).

#### Event

An *event* is just a special kind of cell that can travel across different
flows. In order for a remote recipient to identify the events it receives,
events carry their own type identifiers unique within a single application
domain.

Basically being a cell, an event may contain another cells, thus another events.

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

Please note that a flow is not necessarily a thread. Though we usually use
single-threaded flows to keep things simple, some flows may run multiple threads
simultaneously. Some flows do not even have its own thread so that it can be
embedded into existing applications.

#### Hub

A *hub* is an event distribution bus to which flows are attached. In most cases,
an x2 process maintains only one single hub with it. Once an event is *posted*
up to a hub, every attached flow is notified with it.

While a flow usually performs precise dispatching, a hub performs blind
distribution, meaning that a posted event is pushed into all the attached flows
regardless of whether they subscribed for that specific event or not.

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

### Shared Knowledge

#### Constant

Shared constants may be defined in x2 definition files. Usually x2 event type
identifiers are defined as shared integer constants.

### Structural Components

#### Case

Though flows already provide separate execution units, they are still tightly
coupled with their physical threading models. And it gives us little benefit
when we need to re-organize the overall threading architecture. So we introduce
a *case*, a more logical execution unit, in order to support flexible
re-engineering of threading architecture. In this idea, a flow is considered as
a stack of relevant cases which are initialized and terminated along with their
holding flow.

#### Hub Channel

In some complicated applications, a lot of flows may be attached to a single
hub. When it is clear that some of the events are meaningful to only a limited
set of flows, blindly distributing those events through all the attached flows
is definitely not desirable. Ports may introduce hub channels to handle this
case. If a posted event is assigned to a specific channel, only the flows that
is explicitly subscribing to that channel would be notified with it.
