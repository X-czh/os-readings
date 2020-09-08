# The Performance of µ-Kernel-Based Systems

H. Härtig, M. Hohmuth, J. Liedtke, S. Schönberg, and J. Wolter, SOSP 1997

## Summary

Port Linux to run on L4, a 2nd-gen µ-kernel, and compare it to 1) native Linux 2) MkLinux (Linux running on 1st-gen Mach-derived µ-kernel). L4Linux is much faster than MkLinux (even with co-location). Typical penalties of using L4Linux over native Linux range from 5% to 10%.

## Motivation

1st-generation µ-kernels have reputation for being too slow and lacking sufficient flexibility. Can 2nd-generation µ-kernel (L4) overcome these limitations?

## Experiment: Port Linux to Run on L4

Port Linux to run on L4 and compare it to 1) native Linux 2) MkLinux (Linux running on 1st-gen Mach-derived µ-kernel).

### L4 Essentials

* Based on 2 basic concepts: threads and address spaces.

* Recursive construction of address spaces by user-level servers.

* All address spaces maintained by user-level servers, also called pagers.

### Linux on top of L4

* __Fully Binary Compliant with Linux/x86__.

* __Minimizing Porting Effort__: Restricted modifications to the architecture-dependent part. No Linux-specific modifications to L4 kernel.

* __Linux Server (Kernel)__: Single Linux server task. The server acts as a pager for the user processes it creates.

* __Interrupt Handling__: L4 maps hardware interrupts to messages. Linux top-half handlers implemented as threads waiting for messages.

* __User Processes__: Each Linux user process is implemented as an L4 task.

* __Syscall__: Syscalls implemented using IPCs between user processes and the Linux server.

* __Signaling__: An additional signal-handler thread was added to each Linux user process.

* __Scheduling__: All threads are scheduled by L4's internal scheduler.

## Result

### Compatibility Performance

* Microbenchmark: syscall cost.

* Macrobenchmark: overall system performance.

1. What is the penalty of using L4 Linux instead of native Linux?

    Typical penalties range from 5% to 10% in macrobenchmarks.

2. Does the performance of the underlying µ-kernel matter?

    Yes after comparing two µ-kernel implementations of the same OS.

3. How much does co-location (a faster version of MkLinux uses a co-located server running in kernel mode) improve performance?

    Co-location on its own is not sufficient to overcome performance deficiencies when the basic µ-kernel does not perform well.

### Extensibility Performance

* Pipes and VM operations can be made faster with L4's faster IPC. (-> Why can't macrokernel adopt faster IPC though?)

* Real-time applications need a memory management different from the one Linux implements. L4's hierarchical user-level pagers allows both the L4Linux memory system and a dedicated real-time one to be run in parallel. (-> This is a indeed a selling point.)
