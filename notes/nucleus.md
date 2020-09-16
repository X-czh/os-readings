# The Nucleus of a Multiprogramming System

P. B. Hansen, Communications of the ACM 1970

## Summary

Concept of OS Kernel; Separation of Policy and Mechanism

## Problem

Existing operating systems make rigid assumptions in their basic design about a specific mode of operation. When new needs arise, it is difficult to modify them.

## Goal

Build a new multiprogramming system whose focus is not to define functions that satisfy specific operating needs, but rather to supply a system kernel that can be easily extended.

## Contribution

* Concept of OS Kernel
  * Concentrates on the fundamental aspects of the control of an environment consisting of parallel, cooperating processes
  * Microkernel: program scheduling, resource allocation, etc. implemented in other programs

* Separation of Policy and Mechanism
  * Kernel provides basic mechanism, processes provide policy
