# The Structuring of Systems Using Upcalls

D. D. Clark, SOSP 1985

This paper presents a new methodology for program structure, which consists of two parts: upcalls and multi-task modules. However, I personally find this paper very abstract possibly because of lack of large system engineering experience. I have difficulty understanding some of their proposed pros and cons of the new methodology.

## Motivations

This paper is concerned with the programs modularized according to the principle of layering. Their first observation involves inefficiency arose from the top-down service invocation model. Traditionally, a layer is conceived as providing service to the layer above, or the client layer. Service invocation occurs from top down and may be implemented as a subroutine call. However, the natural flow of control may not be top down, and can be bottom up or sideways as well, particularly in a  network-driven environment. In that case, some inefficient asynchronous mechanisms must used. Their second observation involves the approach to structuring a layer. Traditionally, a layer is implemented as a task or process. However, this requires the communication between layers be via asynchronous inter-process messages, which leads to serious performance issue from the authors' own experience of implementing networking protocols.

## Solution

For the first observation, they proposed that layered systems should be organized in a way that programmers have the freedom to choose whether an upward or sideways control flow can be implemented with a procedural call or asynchronous signals. This feature is called "upcalls". For the second observation, the proposed that a layer in the system shall be implemented as a collection of subroutines which live in a number of tasks, where each subroutine is callable from either above or below. They call subroutines in different tasks that make up a layer a "multi-task module". A multi-task module also contains a collection of state variables shared across the layer via shared memory.

## Advantages of the methodology

* Upcalls
  * Implement upward flow of control by synchronous subroutine call instead of the inefficient asynchronous signals.
  * Simplify implementation by eliminating codes that buffer data at layer boundaries and by allowing one layer to ask for additional useful information from above layers to facilitate its own task.

* Multi-task module
  * Only export subroutine interfaces which are more familiar to programmers, which leads to less threatening layer interfaces.
  * Eliminate the need to code a systemwidely unified format of an intertask message.
  * Allow delay of decisions about how tasks are used till late in the desgin by decoupling each layer to a collection of multi-task subroutines.

## Problems of the methodology

One major problem is that the new concepts are unfamiliar to programmers.

* Upcalls
  * Upcalls may allow propagation of failures from upper layers to lower layers.
  * Upcalls may involve violation of trust between layers.
  * Upcalls together with downward calls may create indirect recursive calls.

* Multi-task module
  * Difficult to handle the interactions between different tasks properly.
  * Difficult to find all of the pieces of the module to change the global state of a module.

The authors also proposed a few techniques to address these problems. However, from my point of view, it is unclear whether the new methodology introduces more benefits than the problems it brings.

## Contributions

* A new methodology to structure layered system.
* An experimental system "Swift" to validate the new methodology.
