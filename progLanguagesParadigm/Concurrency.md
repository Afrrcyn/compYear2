[[COMP26020]]

So what is concurrency?

> "parts" of a "computation" happening at the same time.
> What could be parts and computation?
> ![[Pasted image 20230322201147.png|500]]

Concurrency is everywhere and necessary for high performance (note it's massive within Hardware) and of course useful for software.

***Atomicity*** - computations happen in a single moment in time
***Ordering*** - computations happen in a specific order.

Concurrent Reality:
***Granularity of Atomicity***- computations beyond a certain level take time.
***Interleaving*** - All consistent orderings of computations are possible and have to be considered.

Hardware operations can be assumed to be atomic
- This is not 100% true however it's good enough for now
- Programs therefore do not have to worry about hardware concurrency.

So how does concurrency break a program?

![[Pasted image 20230323085312.png|400]]

These are both connected at they both operate on `a`, but what happens when we call each from their own thread? Well, we don't know...

And remember, if an ordering of instructions is possible, it will eventually happen...

And the problem is, we do not know which thread should run first if we're performing two streams at once... Sequential Semantics alone cannot answer this..

So we have a ***Data Race*** - where we have a non-deterministic output due to the failure to control concurrency.
The program needs to somehow describe the constraints for consistent inter-leavings.

At a high level of abstraction this means looking at the safety and liveness properties.
At a lower level this means the coordination to enforce a consistent partial ordering

Threads - independent streams of computations with a shared state interactions between threads implicit: reading/writing the same data. coordination has to be explicit.
Processes - Each process has it's own internal state, Interactions between threads are done via explicit messaging. Less coordination needed.
Compiler - compiler does all the work.. very hard but easier for functional programming.

### Thread Based Concurrency
- Streams of computations communicating through shared state
- Underlying granularity of atomicity - instructions
- Astronomically large number of possible interleavings even for small programs
- Impossible to predict what's the output without coordination.

Mutual Exclusion 
- Basic Coordination principle for threads - Access shared state only in critical sections, only one thread allowed in each critical section and they're effectively atomic.

So how do we create a critical section?

![[Pasted image 20230323092118.png|500]]

Critical sections
- Waiting time must be limited
- Definitely finite - a critical section has to always complete otherwise this will lead to starvation.
- A critical section should only perform operations that need to be atomic.

Pre/Post-Protocol
problem solved by traditional approaches
- Read and Modify atomically the variables controlling access to the critical section.
- This is not a problem because all processors have instructions that can atomically read and write to memory.

![[Pasted image 20230323202634.png|400]]

![[Pasted image 20230323202737.png|300]]

![[Pasted image 20230323202901.png|400]]

The most basic technique for mutual exclusion is called a ***lock***.

Which controls access to the critical section.

![[Pasted image 20230323203143.png|400]]

![[Pasted image 20230323203304.png|400]]

With locks waiting threads cannot block other threads.

Another is ***Semaphores***

![[Pasted image 20230323203425.png|400]]

***Monitors***
![[Pasted image 20230323203635.png|500]]

### Process-based concurrency
Thread-based concurrency has a low overhead to create and communicate as it's simply memory accesses - this however is not always possible 

This may not be possible when we have distributed systems, multi-node systems, python

And threads may not always be wanted - implicit communication makes it hard to reason about correctness.

How do processes communicate? *Messages*

![[Pasted image 20230323204134.png|400]]

![[Pasted image 20230323204406.png|400]]![[Pasted image 20230323204425.png|400]]

The choice of messaging is ciritcal for concurrency semantics of the program...

![[Pasted image 20230323204516.png|300]]

Remote Procedure calls above is simplest to reason about but very little concurrency. But the send and reply are atomic.

Here the idle time for the processor is slightly shorter...

![[Pasted image 20230323204811.png|400]]

![[Pasted image 20230323204956.png|400]]

Almost no idle time!

![[Pasted image 20230323205027.png|500]]

FOr Async - the software needs to handle when a message is not received...


### High Level Concurrency

- For threads we have mutual exclusion but it's tedious and hard as we have to manually mark all critical sections and it's easy to get wrong.
- Message passing is tedious and adds a lot of overhead...
High level concurrency bugs are still possible 
- Deadlocks - cycle of processes all wait for the nest process in the cycle to leave a critical section.
- Livelocks - cycle of processes all try to enter a critical section but they all fail
- Starvation - processes cannot progress, waiting forever to enter a critical section.

inconsistent states are still possible.

High level concurrency bugs are:
- Hard to identify 
- Hard to debug
- Hard to figure out the sequence of events that lead to the bug


![[Pasted image 20230323211704.png|400]]

Safety - what should also be true!
Liveness - what should eventually become true - shows us if the program progresses or not.

Beyond formal approaches we also have trial and error.

![[Pasted image 20230323211908.png|400]]

![[Pasted image 20230323212123.png|400]]

![[Pasted image 20230323212224.png|400]]

![[Pasted image 20230323212327.png|400]]


