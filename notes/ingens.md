# Coordinated and Efficient Huge Page Management with Ingens

Y. Kwon, H. Yu, S. Peter, C. J. Rossbach, and E. Witchel, OSDI 2016

## Summary

This paper presents Ingens, a framework for huge page support that relies on a handful of basic primitives to provide transparent huge page support in a principled, coordinated way.

## Motivations

Hardware manufacturers have addressed increasing DRAM capacity with better support for huge pages, which reduce address translation overheads by reducing the frequency of TLB misses. However, current operating system memory management has generally responded to huge page hardware with best-effort algorithms and spot fixes, and suffers from multiple problems.

### Current huge page problems (mainly targeting Linux)

1. Page fault latency and synchronous promotion

    Linux greedily and aggressively allocates huge pages, which increases page fault latency. First, Linux must zero pages before returning them to the user. Huge pages are much larger than base pages, and thus are much slower to clear. Second, huge page allocation requires larger physically contiguous memory. When memory is fragmented, Linux will often synchronously compact memory in the page fault handler, increasing average and tail latency.

2. Increased memory footprint

    Linux greedily allocates huge pages even though under-utilized huge pages create internal fragmentation.

3. Huge pages increase fragmentation

    Aggressive promotion of huge pages quickly consumes available physical memory contiguity, which then increases memory fragmentation for the remaining physical memory.

4. Unfair performance

    Unfair huge page allocation (due to fragmentation) can lead to unfair performance differences when huge pages become scarce.

5. Memory sharing vs. performance

    In KVM, identical page sharing in the host is done transparently in units of base pages, and it prioritizes reducing memory footprint over preservation of huge pages, so it penalizes performance.

## Solution

This paper presents Ingens, a memory management redesign that aims to bring performance, memory savings and fairness to memory-intensive applications with dynamic memory behavior. It is based on two principles: (1) memory contiguity is an explicit resource to be allocated across processes and (2) good information about spatial and temporal access patterns is essential to managing contiguity; it allows the OS to tell/predict when contiguity is/will be profitably used.

1. Monitoring space and time

    Measures the utilization of huge-page sized regions (space) and how frequently huge-page sized regions are accessed (time).

2. Fast page faults with asynchronous promotion

    The page fault handler only decides when to promote a huge page and the promotion is done asynchronously.

3. Utilization-based promotion

    Explicitly and conservatively manages memory contiguity as a resource, allocating contiguous memory only when it decides a process (or VM) will use most of the allocated region based on utilization.

4. Proactive batched compaction (reduce fragmentation)

5. Balance page sharing with performance

    Uses access frequency information to balance identical page sharing with application performance.

6. Fair promotion

    When contiguity is contended, fairness is achieved when all processes have a priority-proportional share of the available contiguity.

## Evaluation

Ingens is implemented in Linux 4.3.0, and is evaluated using the a lnumber of memory intensive applications against the performance of Linux’s huge page support. Ingens reduces tail-latency and bloat, while improving fairness and performance.

## Comments

Ingens mainly improves over Linux's huge page support by incorporating a utilization-based promotion algorithm. I also like their idea of separating promotion decisions (policy) from huge page allocation (mechanism), which fastens page fault handling.

The intuition of this paper is clear and it achieves its goal by combining a lot of old ideas (like separation of policy and mechanism, batch compaction) to apply on a new problem. I like it.
