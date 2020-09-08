# Plan 9 from Bell Labs

R. Pike, D. Presotto, S. Dorward, B. Flandrena, K. Thompson, H. Trickey, and P. Winterbottom, USENIX Computing Systems 1995

## Motivation

### Problem

By the mid 1980's, the trend in computing was away from large centralized time-shared computers towards networks of smaller, personal machines, typically UNIX 'workstations'.

However, the rush to personal workstations has two major weaknesses. First, UNIX itself is old and has had trouble adapting to ideas born after it like graphics and networking. Second, timesharing centralized the management and amortization of costs and resources while personal computing fractured, democrafized, and ultimately amplified administrative problems.

### Goal

Plan 9 aims to build a distributed operating system that runs on a system of cheap modern microcomputers and is centrally administered and cost-effective.

## Design

The system is built upon three principles:

1. Resources are named and accessed like files in a hierarchical file system.

2. There is a standard protocol, called 9P, for accessing these resources.

3. The disjoint hierarchies provided by different services are joined together into a single private hierarchical file name space.

Build a UNIX-like system from the network, not a network of loosely connected UNIX systems!

A large Plan 9 installation has a number of computers networked together, each providing a particular class of service. Users work at terminals with screens, keyboards, and mice that acquire most computing and storage resources over the network. Shared multiprocessor servers provide computing cycles; other large machines offer file storage.

### Human Engineering

Plan 9 is meant to be used from a machine with a screen running the window
system. But Plan 9 still has a text-based shell.

### File Servers

A file server in Plan 9 is just something that serves resources with a file-like interface. The resources could be normal files, or networking on `/net`, or processes on `/proc`, or bitmap graphics on `/dev/cons`, etc. With 9P, local and remote resources are treated in a unified way.

### Namespaces

Each process has its own view of the namespace (local namespace). There are known names for services and uniform names for files exported by those services, but the view is entirely local. Resources in another hierarchy can be "mounted" to the existing namespace. Multiple directories can also be stacked at a single
point in the namespace by `bind` or `mount`.

### Security

Authentication (between users rather than machines) is usually done under the auspices of a 9P attach message. Each Plan 9 user has an associated DES authentication key; the user's identity is verified by the ability to encrypt and decrypt special messages called challenges.

Plan 9 has no super user. Each server is responsible for maintaining its own security, usually permitting access only from the console protected by a password.

### Native UTF-8

UTF-8 was invented for Plan 9 to simplify the exchange of text between programs. It has several attractive properties, including byte-order independence, backwards compatibility with ASCII, and ease of implementation.
