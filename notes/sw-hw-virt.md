# A Comparison of Software and Hardware Techniques for x86 Virtualization

K. Adams and O. Agesen, ASPLOS 2006

## Summary

This paper presents a quantitative performance comparison of an existing software VMM with a new VMM designed for the emerging hardware support around the year 2006. It finds that hardware support fails to provide an unambiguous performance advantage due to MMU virtualization problem.

## Motivations

The x86 has historically lacked hardware support for virtualization and software techniques that go beyond the classical trap-and-emulate VMM (e.g. binary translation) have been developed to address this issue. The major x86 CPU manufacturers later introduced architectural extensions to directly support virtualization in hardware. The transition from software-only VMMs to hardware-assisted VMMs provides an opportunity to examine the strengths and weaknesses of both techniques.

## Comparison

The paper carries out experiments with SW-based (binary-translation based) and HW-based VMM of VMware Player on a series of real workloads and benchmarks as well as nano-benchmarks. The findings are:

1. Compute-intensive workload: toss-up
2. System calls: HW VMM wins
3. Process switches, I/O, address space modifications: SW VMM wins

The hardware support fails to provide an unambiguous performance advantage for two primary reasons: first, it offers no support for MMU virtualization; second, it fails to co-exist with existing software techniques for MMU virtualization. However, the HW support is progressing with microarchitectural improvement and MMU virtualization support.

## Epilogue

AMD's nested paging and Intel's EPT arrived right after this.
