# The Linux Scheduler: a Decade of Wasted Cores

J-P. Lozi, B. Lepers, J. Funston, F. Gaud, V. Quéma, and A. Fedorova, EuroSys 2016

## Summary

This paper discovers and analyzes performance bugs of Linux scheduler in the multicore era that cause cores to stay idle for seconds while ready threads are waiting in runqueue.

## Motivations

Scheduling was once considered to be a solved problem in operating systems by the year 2000. However, with the arrival of the multicore era, the pressure to work around the challenging properties of modern hardware makes schedulers incredibly complicated. As a result, the very basic function of the scheduler, which is to make sure that runnable threads use idle cores, is often broken.

## Contribution

The main contribution is the discovery and analysis of four performance bugs in the Linux scheduler. These bugs cause the scheduler to leave cores idle while runnable threads are waiting for their turn to run, resulting in substantial performance degradations and energy waste. Fixes are also provided for the bugs.

Side contributions include the development of an online sanity checker and a scheduler visualization tool that are useful for identifying scheduler bugs and understanding their roots.

## Performance bugs

1. The Group Imbalance bug
    * When a core tries to steal work from another scheduling group (SG), it does not examine the load of every core in that group, it only looks at the group’s average load.
    * If the average load of the victim SG is greater than that of its own SG, it will attempt to steal from that group; otherwise it will not.
    * Average doesn't account for spread. A core might be idle while its SG has high average load.
    * **Fix**: Instead of comparing the average loads, compare the minimum loads.

2. The Scheduling Group Construction bug
    * When an application is pinned on nodes that are two hops apart, load balancing algorithm from migrating threads between these two nodes is prevented.
    * Due to the way scheduling groups are constructed, which is not adapted to modern NUMA (non-uniform memory access latencies) machines.
    * **Fix**: Modify the construction of scheduling groups so that each core uses scheduling groups constructed from its perspective.

3. The Overload-on-Wakeup bug
    * A thread that was asleep may wake up on an overloaded core while other cores in the system are idle.
    * Introduced by an optimization in the wakeup code whose rationale is to maximize cache reuse. It does not payoff in some workloads though.
    * **Fix**: Wake up the thread on the local core (the core where the thread was scheduled last) if it is idle; otherwise, if there are idle cores in the system, wake up the thread on the core that has been idle for the longest amount of time. If there are no idle cores, fall back to the original algorithm to find the core where the thread will wake up.

4. The Missing Scheduling Domains bug
    * When a core is disabled and then re-enabled using the /proc interface, load balancing between any NUMA nodes is no longer performed.
    * Due to an incorrect update of a global variable representing the number of scheduling domains in the machine.
    * **Fix**: Fix the update of the global variable.

## Lessons learned

1. The scheduler infrastructure should be structured as a collection of modules: a core module plus a bunch of optimization modules, so as to allow easy integration of scheduler optimizations (sounds like LLVM for compilers).

2. Visualization tools are crucial for understanding the root causes of the bugs.
