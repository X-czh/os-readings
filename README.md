# Readings in Operating Systems

A list of papers essential to computer software system design with my notes. It is based on the reading lists of [CS 523 Advanced Operating Systems (Fall 2020)](https://cs523-uiuc.github.io/fall20/readings.html) and [CS 423 Operating System Design (Spring 2020)](https://cs423-uiuc.github.io/spring20/) at UIUC.

## Historical Perspectives

* The Structure of the "THE"-Multiprogramming System (1967). [Paper](https://dl.acm.org/doi/10.1145/363095.363143) \| [Note](notes/the.md)

* The Nucleus of a Multiprogramming System (1970). [Paper](https://dl.acm.org/doi/10.1145/362258.362278) \| [Note](notes/nucleus.md)

* TENEX, a Paged Time Sharing System for the PDP-10 (1971). [Paper](https://dl.acm.org/doi/10.1145/361268.361271) \| Note

* HYDRA: The Kernel of a Multiprocessor Operating System (1974). [Paper](https://dl.acm.org/doi/10.1145/355616.364017) \| Note

## Unix and Plan 9 (and MINIX and Linux)

* The UNIX Time-Sharing System (1973). [Paper](https://dl.acm.org/doi/10.1145/361011.361061) \| [Note](notes/unix.md)

* Plan 9 From Bell Labs (1995). [Paper](https://www.usenix.org/legacy/publications/compsystems/1995/sum_pike.pdf) \| [Note](notes/plan9.md)

* Lessons Learned from 30 Years of MINIX (2016). [Paper](https://cacm.acm.org/magazines/2016/3/198874-lessons-learned-from-30-years-of-minix/fulltext) \| [Note](notes/minix.md)

* [Linux's Early History](https://www.cs.cmu.edu/~awb/linux.history.html) by Linus Torvalds (1992).

## Microkernel

* Improving IPC by Kernel Design (1993). [Paper](https://dl.acm.org/doi/10.1145/173668.168633) \| [Note](notes/improving-ipc-by-kernel-design.md)

* The Performance of μ-Kernel-Based Systems (1997). [Paper](https://dl.acm.org/doi/10.1145/269005.266660) \| [Note](notes/perf-microkernel-based-system.md)

## Library OS

* Exokernel: An Operating System Architecture for Application-Level Resource Management (1995). [Paper](https://dl.acm.org/doi/10.1145/224057.224076) \| [Note](notes/exokernel.md)

* Unikernels: Library Operating Systems for the Cloud (2013). [Paper](https://dl.acm.org/doi/10.1145/2490301.2451167) \| [Note](notes/unikernel.md)

## Synchronization

* Experience with Processes and Monitors in Mesa (1980). [Paper](https://dl.acm.org/doi/10.1145/358818.358824) \| [Note](notes/monitor-mesa.md)
  * Background: Monitors: An Operating System Structuring Concept (1974). [Paper](https://dl.acm.org/doi/10.1145/355620.361161)

* Transactional Memory: Architectural Support for Lock-Free Data Structures (1993). [Paper](https://dl.acm.org/doi/10.1145/173682.165164) \| [Note](notes/transactional-memory.md)

* Eraser: A Dynamic Data Race Detector for Multi-Threaded Programs (1997).
  * Finding and Reproducing Heisenbugs in Concurrent Programs (2008). [Paper](https://dl.acm.org/doi/10.5555/1855741.1855760) \| [Note](notes/heisenbug.md)
  * Ad Hoc Synchronization Considered Harmful (2010). [Paper](https://www.usenix.org/legacy/events/osdi10/tech/full_papers/Xiong.pdf) \| [Note](notes/ad-hoc-sync.md)

* Making Parallel Programs Reliable with Stable Multithreading (2014).

## Scheduling

* Scheduler Activations: Effective Kernel Support for the User-level Management of Parallelism (1991). [Paper](https://dl.acm.org/doi/10.1145/121132.121151) \| [Note](notes/sched-activation.md)
  * Background: The Structuring of Systems Using Upcalls (1985). [Paper](https://dl.acm.org/doi/10.1145/323647.323645) \| [Note](notes/upcall.md)

* The Linux Scheduler: a Decade of Wasted Cores (2016). [Paper](https://dl.acm.org/doi/10.1145/2901318.2901326) \| [Note](notes/linux-sched-bug.md)

* Lottery Scheduling: Flexible Proportional-Share Resource Management (1994). [Paper](https://www.usenix.org/legacy/publications/library/proceedings/osdi/full_papers/waldspurger.pdf)  \| [Note](notes/lottery-sched.md)

## Memory Management

* Translation Caching: Skip, Don’t Walk (the Page Table) (2010). [Paper](https://dl.acm.org/doi/10.1145/1815961.1815970) \| [Note](notes/translation-cache.md)

* Elastic Cuckoo Page Tables: Rethinking Virtual Memory Translation for Parallelism (2020). [Paper](https://dl.acm.org/doi/10.1145/3373376.3378493) \| [Note](notes/elastic-cuckoo-page-table.md)

* Efficient Virtual Memory for Big Memory Servers (2013). [Paper](https://dl.acm.org/citation.cfm?id=2485943) \| [Note](notes/direct-segment.md)

* Coordinated and Efficient Huge Page Management with Ingens (2016). [Paper](https://www.usenix.org/system/files/conference/osdi16/osdi16-kwon.pdf)  \| [Note](notes/ingens.md)

## Virtualization

* Xen and the Art of Virtualization (2003). [Paper](https://dl.acm.org/citation.cfm?id=945462) \| [Note](notes/xen.md)

* A Comparison of Software and Hardware Techniques for x86 Virtualization (2006). [Paper](https://dl.acm.org/citation.cfm?id=1168860) \| [Note](notes/sw-hw-virt.md)

* My VM is Lighter (and Safer) than your Container (2017).

## Storage and File Systems

* A Fast File System for Unix (1984). [Paper](https://dl.acm.org/doi/10.1145/989.990) \| [Note](notes/ffs.md)

* The Design and Implementation of a Log-Structured File System (1991). [Paper](https://dl.acm.org/doi/10.1145/121133.121137) \| [Note](notes/lfs.md)

* Soft Updates: A Solution to the Metadata Update Problem in File Systems (2000). [Paper](https://dl.acm.org/doi/10.1145/350853.350863) \| [Note](notes/soft-updates.md)

* The Rio File Cache: Surviving Operating System Crashes (1996). [Paper](https://dl.acm.org/doi/10.1145/989.990) \| [Note](notes/rio.md)

* The Google File System (2003). [Paper](https://dl.acm.org/doi/10.1145/945445.945450) \| [Note](notes/gfs.md)

* Windows Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency (2011). [Paper](https://dl.acm.org/doi/10.1145/2043556.2043571) \| [Note](notes/azure-storage.md)

## Communication

## Distributed Systems

* The Sprite Network Operating System (1988).

* The Distributed V Kernel and its Performance for Diskless Workstations (1983).

* Web Search for a Planet: The Google Cluster Architecture (2003).

* MapReduce: Simplified Data Processing on Large Clusters (2004).

## Performance

## Protection

## Reliability
