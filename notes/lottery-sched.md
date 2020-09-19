# Lottery Scheduling: Flexible Proportional-Share Resource Management

C. A. Waldspurger and W. E. Weihl, OSDI 1994

This paper presents Lottery Scheduling: a lottery-based, general, proportional-share scheduling algorithm.

## Motivations

Traditional schedulers are weak at supporting flexible, responsive control over service rates. Priority-based schemes are often ad hoc, and suffers from the problem of priority inversion (high-priority jobs can be blocked behind low-priority jobs). Existing fair share schedulers and microeconomic schedulers successfully address some of the problems with absolute priority schemes, but are complex and difficult to control.

## Solution

This paper presents Lottery Scheduling:

* Resource rights are represented as lottery tickets. Priority determined by the number of tickets that each process has.

  * Tickets are independent of machine details (**abstract**).
  * Priority is the relative percentage of all of the tickets competing for this resource (**relative**).
  * Tickets can be used homogeneously for heterogeneous resources (**uniform**).

* Scheduler picks winning ticket randomly.

  * Scheduling by lottery is **probabilistically fair**.

The explicit representation of resource rights as lottery tickets further provides a convenient substrate for modular resource management:

* Tickets transfers.

  * Idea: if you are blocked on someone else, give them your tickets temporarily.
  * Solves the priority inversion problem when dependency is involved: Assume program T depends on S. This helps ensure that the S runs with at least the same priority of T, allowing it to be finished quickly for T to proceed.

* Ticket inflation.
  
  * Can be used to adjust resource allocations without explicit communication among trusted clients.

* Ticket currencies.
  
  * Set up an exchange rate with the base currency.
  * Allows ticket inflation with one group.

* Compensation tickets.

  * Idea: if you complete fraction f of the quantum, your tickets are inflated by 1/f until the next time you win.
  * Solve the problem that a client that always does not consume its entire allocated quantum (e.g. an I/O-bound client) would receive less than its entitled share of the processor.

## Evaluation

The scheduling policy was implemented on Mach 3.0 microkernel and tested in experiments against various benchmarks. It is shown that the lottery scheduler could control the relative execution rates of computations with high accuracy in the long run. However, for the multimedia experiment, the control of the relative execution rates of computations is distorted due to the round-robin processing of client requests by the single-threaded server. A particularly interesting experiment is on Flexible Control for the Monte-Carlo algorithm. Scientists frequently execute several separate Monte-Carlo experiments to explore various hypotheses. It is often desirable to obtain approximate results quickly whenever a new experiment is started, while allowing older experiments to continue reducing their error at a slower rate. By dynamically adjusting an experiment’s ticket value as a function of its current relative error, lottery scheduling can easily implement this feature.

## Flaws

* If a process alternates between using up half of the quantum and all of the quantum, it can take advantage of the compensation tickets.
* There is large short-term variation due to random selection of tickets.
* The control of the relative execution rates of computations can be distorted by other forces, as is revealed by the multimedia app experiment.
