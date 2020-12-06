# Soft Updates: A Solution to the Metadata Update Problem in File Systems

G. R. Ganger, M. K. McKusick, C. A. N. Soules, and Y. N. Patt, ACM Transactions on Computer Systems (TOCS) 2000

## Summary

This article describes soft updates, an implementation technique for low-cost sequencing of fine-grained updates to write-back cache blocks. This approach carefully orders all writes to the file system to ensure that the on-disk structures are never left in an inconsistent state.

## Motivations

### The Metadata Update Problem

Several important file system operations consist of a series of related modifications to separate metadata structures. To ensure recoverability in the presence of unpredictable failures, the modifications often must be propagated to stable storage in a specific order.

### Unsatisfactory Previous Solutions

1. Synchronous Writes
    * Problem: poor performance.

2. Nonvolatile RAM (NVRAM)
    * Use memory that can survive a crash or power failure.
    * Problem: requires special hardware support.

3. Atomic Updates
    * Group each set of dependent updates as an atomic operation.
    * Problem: requires changes to the on-disk; complex.

4. Scheduler-Enforced Ordering
    * With appropriate support in disk request schedulers, a file system can use asynchronous writes for metadata and pass any sequencing restrictions to the disk scheduler with each request.
    * Problem: makes writes asynchronous, not delayed, thus does not reduce the number of writes.

5. Interbuffer Dependencies
    * Use delayed writes for all updates and have the cache write-back code enforce an ordering on disk writes.
    * Problem: the system must avoid circular dependencies (e.g., by doing a synchronous write), which quickly arise in the normal course of file system operation

## Solution

Soft Updates: tracks dependencies among updates to cached (i.e., in-memory) copies of metadata and enforces these dependencies, via update sequencing, as the dirty metadata blocks are written back to nonvolatile storage.

### Fine-Grained Dependency Tracking

The blocks that are read from and written to disk often contain multiple structures (e.g., inodes or directory fragments), each of which generally contains multiple dependency-causing components (e.g., block pointers and directory entries). As a result, originally independent changes can easily cause dependency cycles. Consider the example that one file is created and another is deleted, both in the same directory (see page 8).

Solution: maintain dependency information at a very fine granularity: per field or pointer.

## Evaluation

Experiments were carried out in the UNIX context on metadata-update-intensive micro-benchmarks on, overall system performance, and recovery time. It is shown that soft updates gives performance improvements of 100 – 2000% for benchmarks and recovery time improvements of more than two orders of magnitude. It also represents 4 – 19% higher system performance than write-ahead logging.

Seems like a small change, but brings much boosting!
