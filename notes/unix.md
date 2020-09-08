# The UNIX Time-Sharing System

Dennis M. Ritchie and Ken Thompson, Communications of the ACM 1974

## Summary

This paper presents the overall design of the early UNIX systems as well as the creators' insider perspectives on the design choices. Key components like the file system, process control scheme, and the shell are also described in great detail.

## Motivation

As programmers themselves, the authors were not satisfied with the available computer facilities at the time for their programming inconvenience. They wanted to make a system that was convenient for programming with reasonable efficiency under fairly severe size constraints.

## Solution

For programming convenience, UNIX provides an interactive program called the Shell, which is easy to use and also powerful. The interface to file system is designed to eliminate distinctions between ordinary files and various devices and between random and sequential access. Also, restrictions on the data structures within a program's address space is avoided.

Meanwhile, to achieve reasonable efficiency of the system under fairly severe size constraints, the authors were pushed to seek a certain elegance of design. Device-dependent considerations are mostly handled by the operating system itself for space efficiency. The Shell operates as an ordinary, swappable user program, with no wired-down space in the system proper consumed. The framework in which the Shell executes as a process that spawns other processes to perform commands makes the implementation of the system interfaces trivial.

## Evaluation

To evaluate the efficiency of the files system, the authors presented timings of the assembly of a 7621-line program.

To illustrate the scale of the system and show how it is used, selected user statistics were provided, including user population, file size, command CPU usage and access percentage. Brief reliability statistics were also presented.

## Contributions

UNIX does not have many new inventions, but it fully exploits of a carefully selected set of fertile ideas. In an essence, it is a small yet powerful operating system that incorporates

* A simple yet efficient file system that supports demountable volumes and eliminates distinctions between ordinary files and various devices and between random and sequential access.

* A convenient and efficient process control scheme with a set of simple system calls like fork(), pipe(), etc.

* An interactive command interface with I/O redirection, background processes, command files, and many other useful facilities.

## Flaws

The evaluation is too simple in my view. No interpretation of the figures, nor any comparison with other systems were provided for the evaluation of the efficiency of the files system. The reliability statistics are mostly descriptive only and lacks exact figures and detailed analysis.

In terms of the system design, no user-visible locks are provided for concurrent file accesses, which might not be a problem for their use case but is needed in certain cases.

## Takeaways

* Take the hard work away from users to yourself.
* "Salvation through suffering".
* Use your own systems as early as possible.
