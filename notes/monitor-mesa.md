# Experience with Processes and Monitors in Mesa

B. W. Lampson and D. D. Redell, Communications of the ACM 1980

This paper addresses a number of problems that arise with the use of monitors in real systems.

## Motivations

The use of monitors for describing concurrency has been much discussed in the literature. However, when monitors are used in real systems of any size, a number of problems arise:

* Program structure: How should programs be designed when monitors are used?
* Dynamic process creation: Previous work assumed a fixed amount of concurrency, how to do dynamic process creation?
* Dynamic allocation of monitors: How to allow the number of monitors in a program or a module to vary dynamically?
* WAIT in a nested monitor calls: what is the semantics?
* Exceptions: How should the system handle exceptions when monitors are used?
* Scheduling: How should monitors interact with priority scheduling of processes?
* Input-output: How to fit I/O devices into the framework of monitors and condition variables?

## Solution

These problems are addressed by the facilities developed for concurrent programming in Mesa.

### Semantics of WAIT

* Hoare style: A notify awakens one waiter immediately abnd the signaling process in turn runs as soon as the waiter leaves the monitor. The awakened process then knows that notify just occurred, giving more information than just that invariant established.
* Mesa style: Notify is just an hint and awakening may not happen immediately. Awakened process must verify that the condition of interest is true each time it is awoken.

Mesa style results in less context switching and no constraints on when process must run after notify.

### Others

The solutions to the above questions are:

* Program structure:

    Monitor is an object comprised of: Synchronization mechanism; Shared data; Methods accessing the shared data. The module in Mesa is exactly an object, so monitor can be implemented as an instance of module.

* Dynamic process creation:

    Mesa casts the creation of a new process as a special procedure activation that executes concurrently with its caller. It treats a process as a first class value in the language, which can be assigned to a variable or an array element, passed as a parameter, and in general treated exactly like any other value.

* Dynamic allocation of monitors:

    Often we wish to have a collection of shared data objects, each one representing an instance of some abstract object, and we wish to add objects to the collection and delete them dynamically. To allow this in a concurrent program, the straightforward way is to use a single monitor for accessing all instances of the object. However, if the objects function independently of each other for the most part, the single monitor drastically reduces the maximum concurrency that can be obtained. Mesa makes it easy to make multiple instances of the monitor module.

* WAIT in a nested monitor calls:

    WAIT in a nested monitor calls can lead to three patterns of deadlocks.

  * Two processes do wait, each expects signal from other. Solution: it's program bug, debug it.
  * Cyclic calling between two monitors: monitor M calls an entry procedure in N, and N calls one in M, each will wait for the other to release the monitor lock. Solution: impose a partial ordering on resources such that all the resources simultaneously possessed by any process are totally ordered.
  * Implicit cycle: M calls N, and N then waits for a condition which can only occur when another process X enters N through M and makes the condition true. In this situation, N is unlocked, but M is locked so X can't get to it.

* Exceptions:

    Exception handler should release the monitor lock first and then handle the exception.

* Scheduling:

    Priority inversion (high priority process waits for a lock held by a low priority process, and is prevented from running by a middle priority process) can happen. Solution: associate with each monitor the priority of the highest priority process which ever enters that monitor.

* Input-output:

    Communication between process and device (for example I/O device) can be implemented by monitor. Just place a monitor between the process and device. When the device needs attention, it can NOTIFY a condition variable to wake up a waiting process.

## Comments

The practical issues with monitors are profound, e.g. semantics of nested monitor calls, semantics of WAIT. Mesa-specific issues like program structure, dynamic process creation are mostly irrelevant today.
