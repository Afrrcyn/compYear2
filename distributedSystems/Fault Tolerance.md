# Fault Tolerance I

A characteristic of distributed systems that distinguishes them from single-machine systems is the notion of partial failure: part of the system is failing while the remaining part continues to operate, and seemingly correct

Fault tolerance is about enabling the system to remain operational in an acceptable way, despite the occurrence of failure

## The Notion of Dependability in Fault Tolerance

The ability to be fault tolerant is strongly associated with dependable systems, i.e. systems that present the following dependability requirements:

- Availability
- Reliability
- Safety
- Maintainability

Dependability is defined as the trustworthiness of a computing system which allows reliance to be justifiably placed on the service it delivers

## Requirements for Dependability

1. Availability: The probability that the system operates correctly at any given moment
2. Reliability: Length of time that it can run continuously without failure
3. Safety: If and when failures occur, the consequences are not catastrophic for the system
4. Maintainability: How easily can a failed system be repaired

## Meaning of Failure

A system is said to fail when when it cannot meet it’s promises

In particular, if a distributed system is designed to provide it’s users with a number of services, the system has failed when one or more of those services cannot be completely provided.

Building dependable, fault tolerant systems closely relates to controlling faults

A fault tolerant system can provide it’s services even in the presence of faults

For example, by applying error correcting codes for transmitting packets, it is possible to tolerate relatively poor transmission lines and to reduce the probability that an error (a damaged packet) may lead to failure.

## Types of Failures

![Untitled](Fault%20Tolerance%2079594d938ee64cc89baf358d4cb3e2a2/Untitled.png)

The most serious type of failures are associated with arbitrary failures

When an arbitrary failure occurs, clients should be prepared for the worst possible scenario

Example: A server starts producing output that it should never produce, but which cannot be detected as being incorrect

Arbitrary failures are known as Byzantine failures

# Fault Tolerance II

## The Role of Redundancy in Masking Failures

If a system is to be fault tolerant, the best it can do is to try to hide the occurrence of failures from other processes. The key technique for masking failure is redundancy.

<aside>
💡 Redundancy generally means having something that is not strictly necessary, but provides backup or extra functionality in case of failure. In the context of computing systems, redundancy is the replication of system components, such as hardware or software, in order to provide backup functionality in the event of a failure.

</aside>

### Types of Redundancy used in Fault Tolerance

1. Physical Redundancy: A well-known engineering technique, examples can be found in places such as aircrafts that have four engines, but subject to certain conditions can fly on three as well
2. Time Redundancy: An action is performed, if need be, again and again. It is especially helpful when faults are transient and intermittent
3. Information Redundancy: Example send extra bits when transmitting information to allow recovery

### About Redundancy

Redundancy can indeed create problems 

[[Data Replication]]

- Associated with the consistency of data replicas (e.g. all data needs to be updated)
- Should improve (overall) system performance, but keeping replicas consistent can damage performance

But above all, need to make sure that any failure won’t leave our system in an inconsistent (corrupted) state

### Example: A Simple Application

**A client communicating with a remote server**

Goal: Transfer £100 from account 1 to account 2

1. `x = read_balance(1);`
2. `y = read_balance(2);`
3. `write_balance(1, x - 100);`
4. `write_balance(1, y + 100);`

However we should be aware that crashes (system faults) can occur at any time during the execution, what potential problems can arise because of this?

---

If a process encounters an error during execution, it should terminate the process i.e. not go forward with the rest of the transaction. It should only go forward if there are fault redundancy in place that do not affect the upcoming processes.

## ACID Properties

[ACID Properties](https://www.notion.so/ACID-Properties-24a76a4196ab45bd8358008a6a853a01) 

Atomicity, Consistency, Isolation, Durability

### All-or-Nothing: The need for Atomicity

Either all operations execute or none of them

1. `x = read_balance(1);`
2. `y = read_balance(2);`
3. `write_balance(1, x - 100);`
4. `write_balance(1, y + 100);`

The sequence of operations must execute as an atomic operation i.e. they should either work or not work. If they do not work, the program should terminate. All four processes above should succeed in order for the program to succeed. If one of the operations fail, we should not allow next operations to be processed.

Multiple users can be transferring funds simultaneously, what can go wrong with this?

![Untitled](Fault%20Tolerance%2079594d938ee64cc89baf358d4cb3e2a2/Untitled%201.png)

Difference in reality as the two transfers conflicted or got into each other’s way

### Isolation Execution - The Need for Isolation

We must ensure that ‘concurrent’ applications do not interfere with each other

Interfere: Operations of one user should be kept separate from other user’s requests. For example if one user is accessing a shared space then no other user should be allowed to access that shared space.

**Serial (Sequential) Execution**

Easy to implement but not scalable 

Concurrent executions do not interfere with each other if their execution is equivalent to a serial one: 

- The reads and writes get the same result as if the transfers happened one at a time (i.e. they do not interleave)

Simple but naive solution:

- One transfer at a time
- Not scalable and very slow

How do we maximise concurrency without corrupting data?

### Durability

Updates are persistent once the application successfully completes

In simpler words, durability ensures that data is saved only if the transaction has been completed

### Consistency

An application should not violate a database’s integrity constraints

Example:

- Balance of every customer should not exceed their overdraft limit
- All account holders must have a name and address (verified)

![Untitled](Fault%20Tolerance%2079594d938ee64cc89baf358d4cb3e2a2/Untitled%202.png)

## Transactions

![Untitled](Fault%20Tolerance%2079594d938ee64cc89baf358d4cb3e2a2/Untitled%203.png)

### How are transactions implemented?

Transactions are generally implemented serially

- Managing multiple simultaneous users
    - Concurrency control algorithms
        
        Ensure that execution is equivalent to a ‘serial’ execution (key assumption: transactions have a short duration in the order of milliseconds: you don’t want to block other transactions for too long)
        
- Durability
    - Recovery algorithms
        
        Replay the actions of committed transactions and undo the effects left behind by aborted transactions
        

### Concurrency Control

Two Phase locking (2PL):

1. “Acquire lock” phase (Growing)
    - Get a read lock before reading
    - Get a write lock before reading
    - Read locks conflict with write locks
    - Write locks conflict with read and write locks
2. “Release locks” phase (when the transaction terminates) (Shrinking)
    - Commit or abort

![To better understand the processes](Fault%20Tolerance%2079594d938ee64cc89baf358d4cb3e2a2/Untitled%204.png)

To better understand the processes

**Summary of Two Phase Locking:**

In the growing (acquire) phase, a transaction acquires locks on all the resources it needs to complete its work. Once a lock has been acquired, no other transaction can acquire a conflicting lock on the same resource until the first transaction releases the lock. This prevents two transactions from accessing the same resource simultaneously and potentially causing conflicts or inconsistencies.

In the shrinking (release) phase, a transaction releases all the locks it has acquired, allowing other transactions to acquire the resources they need. Once all locks have been released, the transaction is said to have committed and its changes are made permanent.

The two-phase locking protocol ensures serialisability, which means that the results of executing multiple transactions concurrently are equivalent to the results of executing them in some serial order. This ensures that the database remains consistent even in the presence of concurrent transactions.

### When Replication of Processes is Used

In fault tolerance, redundancy is also associated with the replication of processes 

- Organised replicated processes into a group helps to increase fault tolerance

However there is a price to pay, namely a potential loss of performance

- Most existing solutions require exchange of numerous messages between processes

A key approach to tackle faulty processes is to organise several identical processes into a group

A key property that all groups have is that when a message is sent, all members of the group receive it. In this way, if one process fails in the group, some other processes can then take over it

Process / groups can be dynamic in nature, can create more or destroy groups

A process can join or leave a group during system operations

A process can also be a member of several groups at the same time

### Example - The Price of Process Replication

Consider a distributed transaction

- i.e. involving a number of processes each running on a different machine

The two-phase commit algorithm consists of the following two phases, each consisting of two steps:

![Untitled](Fault%20Tolerance%2079594d938ee64cc89baf358d4cb3e2a2/Untitled%205.png)