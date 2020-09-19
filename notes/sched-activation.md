# Scheduler Activations: Effective Kernel Support for the User-Level Management of Parallelism

T. E. Anderson, B. N. Bershad, E. D. Lazowska, H. M. Levy, SOSP 1991

This paper argues that kernel threads are the wrong abstraction for supporting user-level thread management and describes a kernel interface and a user-level thread package that together provide the functionality of a kernel-thread systems and the performance and flexibility of a user-thread system. The idea is actually similr to exokernel, but it is restricted to scheduling, not that aggressive as exokernel.

## Motivations

Threads can be implemented either at user-level or at kernel-level, but neither approach provides fully satisfactory performance, hence a dilemma.

User-level threads are efficient and flexible without any kernel modification. However, in a real-world environment, user-level threads can exhibit poor performance or even incorrect behaviors due to factors like multiprogramming, I/O, and page faults. Kernel-level threads have system level support but are too heavyweight because of the overhead of system call and not felxibile by enforcing one single underlying implementation.

Therefore, it is desirable to have a solution that gets the advantages from both sides without suffering the disadvantages.

### Running user threads on kernel threads? Poor Integration!

Kernel threads are the wrong abstraction for supporting user-level thread management. There are two related characteristics of kernel threads that cause difficulty:

1. Kernel events, such as processor preemption and I/O blocklng and resumption, are handled by the kernel invisibly to the user level.

2. Kernel threads are scheduled obliviously with respect to the user-level thread state.

Problems:

* One kernel thread per processor: User-level thread blocks, so does kernel thread -> processor idle!
* More kernel threads implicitly results in kernel scheduling of user-level threads -> kernel may de-schedule a thread at a bad time!

## Solution

This paper describes a kernel interface and a user-level thread package that together provide the functionality of a kernel-thread systems and the performance and flexibility of a user-thread system. The main point is to **allow coordination between user and kernel schedulers**.

Each user-level thread system is provided with its own virtual multiprocessor. The kernel has complete control over the process allocation of each address space's user-level threads. Each address space's user-level thread system has complete control over which threads to run on its allocated processors. The kernel notifies an address space whenever the kernel changes the number of processor assigned to it or a user-level thread blocks or wakes up in the kernel, which gives control of thread scheduling to the user-thread system itself. The kernel's role is to vector events to the address space for the thread scheduler to handle, rather than to interpret these events on its own.The address space notifies the the kernel when it want to adjust the number of processors, which allows the kernel to correctly allocate processors on need. In the end, the whole thing is transparent to the application programmer.

### Scheduler Activations: Vessels for Runnig User-Level Threads

* One scheduler activation per processor assigned to address space. Also created by kernel to perform upcall into application's address space (see upcalls below).

* Two stacks — one mapped into the kernel and one mapped into the application address space. The
kernel stack is used whenever the user-level thread running in the scheduler activation’s context executes in the kernel, for example, on a system call.

* Each user-level thread is allocated its own stack when it starts running. When a thread blocks on a user-level lock or condition variable, the thread scheduler can resume running without kernel intervention.

* The crucial distinction between scheduler activations and kernel threads: Once an activation’s user-level thread is stopped by the kernel, the thread is never directly resumed by the kernel. Instead, a new scheduler activation is created to notify the user-level thread system that the thread has been stopped. The scheduling decsion is left to user-level thread libraries.

### Upcalls and Downcalls

* Upcalls:
  * New processor available
  * Processor preempted
  * Thread blocked
  * Thread unblockedn

* Downcalls:
  * Need more processors
  * Processor idle
  * Preempt a lower priority thread
  * Return unused activations

## Evaluation

Topaz (an OS) and FastThreads (a user-level thread package) are modified to implement scheduler activations.

### Microbenchmark 1: User-Level thread Operations

The latencies of 2 thread operations are measured: Null Fork and Signal-Wait. The new system significantly outperforms kernel threads, while slightly slower than the FastThreads package due to the overhead to provide temporary continuations of critical sections when a user-level thread is preempted or blocked.

### Microbenchmark 2: Upcall Performance

Five times slower than Topaz threads. They attributed it to not tuning the performance (?).

### Macrobenchmark: A Solution to N-body problem

Further evaluation is carried out on application performance, and the new system outperforms both the kernel threads implemented by Topaz and the user threads implemented by FastThreads package. Only one application, not quite convincing.
