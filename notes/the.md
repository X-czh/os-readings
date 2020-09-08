# The Structure of the "THE"-Multiprogramming System

E. W. Dijkstra, SOSP 67

## Summary

This paper presents a multiprogramming system with a hierarchical levels of abstractions implemented. It also conveys the author's philosophy with regards to system design, claiming the profitability of a hierarchical system design which makes it easy to perform formal logic reasoning about the correctness of the system.

## Motivation

The author's team wanted to build a system that would process multiple user programs smoothly as a service to the University."THE"-Multiprogramming System was built with the following set of objectives:

* reduced turn-around time for short-during programs
* economic use of peripheral devices
* automatic control of backing store with economic use of the central processor
* multiple applications that use the system at the same time must use it economically
* not intended as a multi-access system.

In addition, they wanted the system to be "flawless" by adopting a systematic way of system building.

## Solution

The total system admits a hierarchical structure with 5 levels of abstractions. Level 0 abstracts the CPU, level 1 abstracts the memory, level 2 abstracts the console I/O, level 3 abstracts the general peripheral I/O, level 4 is independent-user programs, and level 5 is the operator. At level 0, elementary CPU sharing mechanism is implemented, with real-time clock used to prevent any process from monopolizing process power. At level 1, the idea of page and segment (presents a user's view of memory) are separated, and mechanisms to deal with page swapping and page missing are proposed. Above each level, all things beneath the level are presented in a nice abstracted way, which makes it easy for upper level development.

The hierarchical structure also allows for formal logic reasoning about the correctness of the system. When building the system, formal verification is applied after each level is built, and the next level cannot be built until the previous level is proved to be correct.

To deal with concurrency, the paper also proposes semaphores as a way for ensuring mutual exclusion

## Contributions

* hierarchical OS structure
* practice of formal reasoning about system correctness
* semaphores as a way to achieve mutual exclusion
