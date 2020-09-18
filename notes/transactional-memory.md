# Transactional Memory: Architectural Support for Lock-Free Data Structures

Maurice Herlihy and J Eliot B. Moss, ISCA 1993

## Summary

This paper introduces transactional memory, a new multiprocessor architecture intended to make lock-free synchronization as efficient (and easy to use) as conventional techniques based on mutual exclusion.

## Motivations

On one hand, conventional techniques based on mutual exclusion suffer from the following problems:

* **Priority inversion** can occur when a lower-priority process is preempted while holding a lock needed by higher-priority processes.
* **Convoying** can occur when a process holding a lock is descheduled. When such an interruption occurs, other processes capable of running may be unable to progress.
* **Deadlock** can occur if processes attempt to lock the same set of objects in different orders.

On the other hand, experimental software implementations of lock-free concurrent data structures do not perform well as well as their locking-based counterparts.

## Solution

This paper introduces transactional memory, a new multiprocessor architecture intended to make lock-free synchronization as efficient (and easy to use) as conventional techniques based on mutual exclusion.

Transactional memory provides a high-level abstraction that allows a group of load and store instructions to execute in an atomic way (like a transaction in database systems), intended to replace short critical sections. It is implemented by modifying standard multiprocessor cache coherence protocols and providing new primitive instructions for accessing memory and manipulating transaction state.

## Evaluation

The transactional memory is implemented on the Proteus simulator, an execution-driven simulator system for multiprocessors, and tested on three simple benchmarks against four other mechanisms. The simulation showed that it outperforms other techniques for atomic updates.

However, in the simulation, each access to the regular or transactional cache, including transaction commit and abort, is counted as a single cycle, which I think is infeasible in reality.

## Comments

Transactional memory provides hardware support for a high-level lock-free abstraction as an alternative to conventional techniques based on mutual exclusion. However, the implementation in this paper relies on the assumption that transactions have short durations and small data sets, and it's unclear how the design of transactional memory can be extended to more general use cases. Also, the simulation result is a bit unconvincing for the aggressive assumption that each access to the regular or transactional cache takes only one cycle. It is unclear whether transactional memory can be that efficient once it is implemented on real-world architectures.
