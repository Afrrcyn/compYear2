# Sequential vs Multi-Processing

# Multi-Processing I

## A Simple Machine

A machine makes available computing and storage resources

By computing resource we mean, e.g. a processor, such as CPU

By storage resource we mean, e.g., a given amount of primary memory

![Untitled](Sequential%20vs%20Multi-Processing%207d40372c5a2942ee9134734abea40883/Untitled.png)

As shown in the figure, computing resources are represented by the machine CPU and storage resources are associated with the availability of storage space from the machines memory, also known as primary storage. This functionality is provided by the machine’s operating system. In other words, it is the operating system that decides how much computing resources and how much of the storage space goes to a process. 

With functions to read and write data to and from memory, data objects are written to and accessed from memory by programs running on the machine. The location of the data objects in memory is determined by an address.

## Process

A process is an instance of a computer program that is executing on a program

In simpler terms, a process is an executing instance of a program

Resource usage is typically controlled by an operating system (OS)

Since resources are scarce and differ in their capabilities, an OS aims to make the most efficient use possible of those resources

When a process is created, the operating system gives the process a unique identifier and controls how a process is granted access to computing resources.

The operating system also controls the process’s access to a storage resource, by assigning to it an address space

When the OS ensures that each process P has a single address space A that is exclusive to P, we are allowed a sequential reading of the steps that comprise the process

![Untitled](Sequential%20vs%20Multi-Processing%207d40372c5a2942ee9134734abea40883/Untitled%201.png)

## Sequential Reading of the steps that Comprise the Process

![Untitled](Sequential%20vs%20Multi-Processing%207d40372c5a2942ee9134734abea40883/Untitled%202.png)

`Foo bar` executes as a process

The function `Foo bar` manipulates two locations in it’s own address space, which are the locations of variables $x$ and $y$

Suppose we have expected values of $x$ and $y$, it is the responsibility of the OS to ensure that no other process tampers with $x$ and $y$, and that only `Foo bar` has access to $x$ and $y$ at one time

Also, even if many applications are running, if we call `Foo bar` we should expect the results of $x$ and $y$ to come out correctly, according to the input parameters given

## Limitations

What if processing to be done is more than just a simple sum?

What if `foo` and `bar` take long to run?

The processor may have to wait on slow peripheral usage in one or both of them, for example

Can we run them concurrently to save time and resources? Is this a state of deadlock where other processes are waiting for the completion of the previous processes?

## Sequential Processing

What if instead of addition, the functions applied to $a$ and $b$ were more complex, for example:

- more computationally demanding than a single machine can cope well with?
- not available locally in the machine where `foobar` is running

If `foobar` is taking too long to run, we may wish to run them concurrently, in different processes, and perhaps even better, in parallel, in different processors

- If foo and bar are proprietary services held in remote machines, we may not be able to hold local copies of them

> Sequential, isolated processing is simple, but bounded and **limited**
> 

## **************************************************Non Sequential Processing**************************************************

Non sequential, non-isolated processing expands the bounds and limits with respect to performance

Performance is improved as:

- Multiple steps or statements in a sequential process can be executed at the same time, so overall execution time becomes shorter
- Performance also improves as more processes can be completed sooner

It is possible to switch between a process P and a process P' if, for example, P is idle waiting for something (like I/O) to complete; also, we get more responsiveness, as:

- While printing a long document, your machine still allows you to go on doing other things.
- While downloading a file, a web browser still allows you to traverse a link.
- While you ponder what to do next, the same machine goes about attending to someone else.

# Multi-Processing II

Multi-processing is processing via multiple processing units. It is achievable involving single processor and multiprocessor machine architectures . In typical single processor machines, multi-processing is about allowing multiple processes to share resources, under the control of the operating system, each having it’s own address space and because there is only one processor, simultaneous execution of two or more processes is possible by means of a multi-core processor

![Untitled](Sequential%20vs%20Multi-Processing%207d40372c5a2942ee9134734abea40883/Untitled%203.png)

## Multi-Processing vs Multi-Tasking

These are two different concepts

An OS multi-tasks by:

- allowing more than one process to be underway by controlling how each one makes use of the resources allocated to it
- implementing a scheduling policy, which grants each active process a time slice during which it can access the resources allocated to it
    
    In other words, it allows a group of processes to be active concurrently
    
    ![Untitled](Sequential%20vs%20Multi-Processing%207d40372c5a2942ee9134734abea40883/Untitled%204.png)
    

In multitasking, processes are not really executing concurrently

Real concurrency is only possible if the machine has multiple computing resources that the operating system can make use of, otherwise concurrent execution is only apparent and a consequence of an effective scheduling policy.

If all processes get a fair share of the resource, and they get it sufficiently often, it seems to users that all processes are executing concurrently

For example, while a process P is waiting on a slow output device, the OS may schedule another process P’ to make use of the CPU. It seems to users that the machine is both printing for P and running P’

## Multi-Processing by Forking

Forking is a function of the operating system and it generates a copy of the process over which the operating system call was made and it results in two copies to be active concurrently

We call one of the copies a child process and the other a parent process

The child process is given a copy of the parent process’s address space. The address spaces however are distinct. And if so, if either process modifies a variable in it’s address space, this change is not visible to the other process

The child process starts executing after the operating system call, whereas the parent can continue or wait for the child to execute

Ultimately, the parent must mean to find out how and when the child completes execution

![Untitled](Sequential%20vs%20Multi-Processing%207d40372c5a2942ee9134734abea40883/Untitled%205.png)

### Where is Forking used?

Forking is quite common in the client-server type of distributed computing, as a server typically forks a child process for each request it receives

Due to the amount of copying involved, forking is expensive. However modern OS have strategies that make the actual cost quite affordable

Because child and parent processes have distinct address spaces, forking is considered to be safe with regards to consistency of execution outcomes

Discipline in adhering to best practice is nonetheless required (e.g. to avoid zombie processes, to avoid unintended sharing of references to files, etc)

<aside>
💡 Zombie Process:

A zombie process is a process that has completed execution but still has an entry in the process table. This can occur when a parent process fails to wait for the completion of its child process using the `wait` system call. The `wait` system call allows the parent process to retrieve the exit status of the child process and free its resources. If the parent process does not wait for the child process to complete, the child process becomes a zombie process.

</aside>

## Multi-Processing by Threading

Forking imposes a certain degree of isolation. And so, if parent and child need to interact and share (resources?), threading may be a better approach to multi-tasking

Threading differs to forking by the fact that the address space of the parent process is not copied, instead it is shared with a child process or processes. 

This means that if one process changes a variable, all other processes see it

This makes threading less expensive but also less safe than forking

![Untitled](Sequential%20vs%20Multi-Processing%207d40372c5a2942ee9134734abea40883/Untitled%206.png)

## Concurrent Computing

Concurrent computing typically involves a single multi-core processor and an operating system controlling the access to resources by multiple applications at the same time

In some cases, when the number of active applications is higher than the number of machine cores, the operating system can control access to the processor by granting slices of processor time to processes. But if everything goes according to schedule, users will not notice any idle active swap

The improvement in performance gained by the use of a multi-core processor depends very much on the software algorithms used and their implementation in particular possible gains are limited by the fraction of the software that can run in parallel simultaneously on multiple cores.

Processes in concurrent computing are often threads, and so the operating system schedules the execution of *n* copies of a process $P_i$ to run in the same processor, typically sharing a single address space

> In computing, a thread is a sequence of instructions that can be executed independently by a single computing core. Multiple threads can be executed concurrently, each performing a separate task. Threads share the same memory space and can communicate with each other, making them useful for parallel processing and multitasking.
> 

## Parallel Computing

Parallelism is a specific kind of concurrency where tasks are executed simultaneously. And so, multiple processors connected via an interconnect can run many processes at the same time. Not just multiple threads, but really multiple distinct processes

There is truly many processes running at the same time, not just multi threading, but true parallelism

In case of multiple threads, the $n$ copies of a process $P_i$ ($1 \le \ i \le \ n$) can each run in one of $m$, ($1 \le j \le m$) different processes $C_j$, possibly (but not necessarily) sharing a single address space $A$

## Distributed Computing

In distributed computing, we have multiple machines which may involve multiple core processors. The machines are independent, self-sufficient, autonomous, heterogenous machines

There is a spatial separation between the machines which is connected via a network, therefore message exchange between machines is necessary for communication between them and that work effects are failed. The complexity of this type of computing is so high that applications are not written against operating system services, but rather against a middleware API. The middleware then takes some of the complexity upon itself

> Spatial separation refers to the physical distance between two or more objects or entities. In the context of distributed computing, it refers to the physical separation of multiple machines that are connected via a network. These machines may be located in different parts of a building, a city, or even a different country. The physical distance between these machines can have an impact on the speed and efficiency of communication between them.
> 

The $n$ (not necessarily identical) processes $P_i$ $(1 \le \ i \le \ n)$, each run in one of $m$ ($1 \le j \le m)$, different machines $M_j$, that cannot share a single address space $A$ (and therefore must communicate)

## Difference between Multi-Tasking, Multi-Threading, Multi-Processing

**Multi-tasking** refers to the ability of an operating system to allow multiple processes to be underway by controlling how each one makes use of the resources allocated to it. Essentially, an OS grants each active process a time slice during which it can access the resources allocated to it, allowing a group of processes to be active concurrently.

**Multi-threading** involves a sequence of instructions that can be executed independently by a single computing core. Multiple threads can be executed concurrently, each performing a separate task. Threads share the same memory space and can communicate with each other, making them useful for parallel processing and multitasking.

**Multi-processing** is processing via multiple processing units. In typical single processor machines, multi-processing is about allowing multiple processes to share resources, under the control of the operating system, each having its own address space. Because there is only one processor, simultaneous execution of two or more processes is possible by means of a multi-core processor.

In summary, multi-tasking is about allowing multiple processes to be active concurrently, multi-threading involves executing multiple threads of a process concurrently, and multi-processing involves processing via multiple processing units.

AI: 

Deadlock is a situation that occurs when two or more processes are waiting indefinitely for an event that can only be triggered by one of the waiting processes. Deadlocks can occur when two or more processes are competing for the same resources, such as a shared memory location or a file that is being accessed by multiple processes.

To prevent deadlocks, operating systems typically use a combination of techniques, such as resource allocation policies, deadlock detection algorithms, and deadlock prevention methods. Some common prevention methods include avoiding circular dependencies, using a hierarchical ordering of resources, and implementing timeouts to force processes to release resources after a certain period of time. Additionally, some operating systems use a combination of prevention and detection methods to ensure that deadlocks are detected and resolved as quickly as possible.