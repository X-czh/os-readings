# Firecracker: Lightweight Virtualization for Serverless Applications

A. Agache, M. Brooker, A. Florescu, A. Iordache, A. Liguori, R. Neugebauer, P. Piwonka, and D. M. Popa, NSDI 2020

## Summary

This paper presents Firecracker, a new open source Virtual Machine Monitor (VMM) specialized for serverless workloads, but generally useful for containers, functions and other compute workloads within a reasonable set of constraints.

## Motivations

Serverless containers and functions are widely used for deploying and managing software in the cloud. The economics and scale of serverless applications demand that workloads from multiple customers run on the same hardware with minimal overhead, while preserving strong security and performance isolation. The traditional view is that there is a choice between virtualization with strong security and high overhead, and container technologies with weaker security and minimal overhead. This tradeoff is unacceptable to public infrastructure providers, who need both strong security and minimal overhead.

## Solution

This paper presents Firecracker, a new open source Virtual Machine Monitor (VMM) specialized for serverless workloads, but generally useful for containers, functions and other compute workloads within a reasonable set of constraints.

Firecracker uses the Linux Kernel's KVM virtualization infrastructure to provide minimal virtual machines (MicroVMs), supporting modern Linux hosts, and Linux and OSv guests. Firecracker provides a REST based configuration API. It provides limited device emulation for disk, networking and serial console, reflecting its specialization for containers and function workloads. It also provides configurable rate limiting for network and disk throughput and request rate, which is enforced to strongly control the amount of VMM and host kernel CPU time that a guest can consume as the guest is not trusted to implement its own limits. One Firecracker process runs per MicroVM, providing a simple model for security isolation.

## Evaluation

Experiments for Firecracker on Boot time, memory overhead, IO performance are reported. Firecracker is also tested on real workload on AWS for two years. Firecracker is able to run thousands of MicroVMs on the same hardware, with overhead as
low a 3\% on memory and negligible overhead on CPU. It also supports fast switching time and soft allocation. No compatibility issues have been found so far. Block IO and network performance have some scope for improvement, but are still adequate for the current needs.

## Takeaways

The major point is to specialize for a particular working load, and can thus reducing overheads needed by a general solution. It does make sense in public cloud industry where the economic of scales works impressively, and the cost by specialization is negligible.
