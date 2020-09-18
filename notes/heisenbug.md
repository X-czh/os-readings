# Finding and Reproducing Heisenbugs in Concurrent Programs

M. Musuvathi, S. Qadeer, T. Ball, G. Basler, P. A. Nainar, and I. Neamtiu, OSDI 2008

## Summary

This paper presents a tool called CHESS to find and reproduce hard-to-find concurrency bugs in a systematic and deterministic way by taking control over the scheduling of threads and asynchronous events and systematically exploiting all possible thread interleaving.

## Motivations

Concurrency is pervasive in large systems, but it often results in bugs that are extremely difficult to reproduce and eliminate. Even worse, small perturbations to the system (like adding debug statements) may leads failure of reproducing the bug.  Traditional testing methods usually rely on artificially “stressing” the system to get more complete interleaving coverage of thread execution so that concurrency bugs can be revealed. However, this is quite expensive and not complete. Meanwhile, attempts for complete search of concurrency bugs suffer from the problem of state-explosion.  Therefore, it is desirable to have a tool that can do a systematic yet efficient search of concurrency bugs and be easily integrated with existing testing frameworks.

## Solution

This paper presents a tool called CHESS. When attached to a concurrent program, CHESS takes complete control over the scheduling of threads and asynchronous events. It further systematically exploits possible thread interleaving, thereby capturing the interleaving nondeterminism in the program. This allows CHESS to find in simple configurations errors that would otherwise only show up in more complex configurations.

To address the problem of introduced perturbation, CHESS only introduces one extra layer, a thin wrapper layer between the program under test and the concurrency API.

To address the problem of state-explosion, CHESS explores thread schedules giving priority to schedules with fewer preemptions utilizing the intuition called preemption bounding (many bugs are exposed in multithreaded programs by a few preemptions occurring in particular places in program execution).

## Evaluation

The tool was evaluated on three real systems in industry and successfully detected many deeply-hidden bugs. An analysis of example bugs caught by the tools is also presented in the paper. Furthermore, the tool was validated against stress testing and falsifies the assumption held by many people that concurrency bugs can't be find by a small number of threads.

## Comments

The paper presents a useful tool for finding concurrency bugs in a systematic and deterministic way and validates it by catching bugs in real industry programs. The method seems very promising. However, it is basically a state-reduction search algorithm for finding concurrency bugs and the intuition behind the state-reduction (preemption bounding) definitely needs more careful examination. I wish to see more follow-ups of the application of this tool to real-world programs.
