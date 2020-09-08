# Improving IPC by Kernel Design

Jochen Liedtke, SOSP 1993

## Motivation: The IPC Dilemma

* Inter-process communication (IPC) by message passing is central for microkernels, which are good for better modularity, security, etc. It is also the key to distributed systems.

* But, IPC is slow (typ. requiring 100 µs for a short message transfer on a processor running with 50 MHz clock rate).

## Making IPC Faster

100 µs -> 5 µs. Theoretical bound: 3 µs.

Main principles: IPC performance prioritized; consider synergetic effects; cover all levels from architecture down to coding.

### Architectural Level

#### 1. Avoid Syscalls

Introduce synchronous syscalls `call` and `reply & receive next` (besides the non-blocking `send` and `receive`), allowing one syscall per IPC.

#### 2. Complex Message Support

A sequence of send operations can be combined into a single one, if no intermediate reply is required. In their system, one message may contain a direct string (mandatory), indirect strings (optional), and memory objects (optional).

#### 3. Direct Transfer by Temporary Mapping

* _Problem_: Most existing microkernels transfer messages between address spaces by a twofold copy: user A space -> kernel space -> user B space. Directly sharing user level memory of A and B introduces security problems.

* _Solution_: Temporary mapping: the kernel determines the target region of the destination address space, maps it temporarily into a communication window in the source address space, and then copies the message directly from the sender’s user space into its communication window. The communication window is only kernel accessible.

#### 4. Strict Process Orientation

Threads that are temporarily running in kernel mode are handled in the same way as when running in user mode. One kernel stack per thread is allocated.

#### 5. TCBs in Virtual Array

All TCBs are held in a large virtual array located in the shared part of all address spaces. => permits fast TCB access by offset addressing; saves TLB misses; locking a thread can be easily done by unmapping its TCB; helps to make threads persistent; memory management becomes transparent to IPC.

### Algorithmic Level

#### 1. Thread Identifier

* _Problem_: A TCB address can easily be calculated from the thread number. In user mode, however, a thread is always addressed by its unique identifier (UID). => Need quick calculation of thread identifier.

* _Solution_: The UID contains the thread number in its lower 32 bits in such a way that only anding it with a bit mask and adding the TCB array’s base address is necessary.

#### 2. Handling Virtual Queues

* _Problem_: The kernel handles a variety of thread queues, need to avoid page faults when parsing the queues.

* _Solution_: Use doubly linked lists, where the links are held in the TCBs. Unmapping a TCB includes removal from all these queues.

#### 3. Timeouts And Wakeups

* _Problem_: Timeouts can be specified in each IPC operation. Far more ipc operations succeed than fail due to timeout, insertion into and deletion from the wakeup queue must be very fast. A large array indexed by thread number leads to a long parsing time.

* _Solution_: Use a set of n wakeup lists. A thread with wakeup time r is linked into the list r mod n. If a total of k threads are
contained in the wakeup lists, the scheduler will have to inspect k/n entries per clock interrupt, on average.

#### 4. Lazy Scheduling

* _Problem_: using `call` and `reply & receive next` requires some scheduling actions and thus many queue operations.

* _Solution_: just avoid queue manipulation and change only the thread state variable in the TCB.
  * Ready queue contains **at least** all ready threads
  * Wait queue contains **at least** all waiting threads
  * Whenever a queue is parsed the scheduler removes all threads that no longer belong in it ("lazy").
  * Only add thread to ready queue on `send`, end of timeslice and hardware interrupt (the only cases where thread loses processor control and still remains ready).

#### 5. Direct Process Switch

* For IPCs, transfer control directly to called thread: donates timeslice.

* However, when B sends a reply to A
and another thread C is waiting to send a message to B (polling B), C’s IPC to B is immediately initiated before continuing A: for fairness.

#### 6. Short Messages via Registers

* Usually, a high proportion of messages are very short. Transferring short messages via registers seems profitable.

* But hardware dependent.

### Interface Level

* Avoid unnecessary copies.

* Use registers for IPC parameter passing whenever possible.

### Coding Level (Kernel)

* Keep the frequently used kernel code small.

* Use the cache intelligently.

* Using hardware-specific features when possible.

* Avoid loading segment registers.

* Avoid jumps and checks.

## Remarks

* A collection of engineering tricks.

* Systematic approach: start from theoretical bound, cover all levels.

* Not mentioned how is the performance compared with Unix kernels.
