# Data Replication

# Data Replication I

Replication is used to: 

- Enhance system readability
- Improve performance

In addition, data replication can potentially improve system scalability

> Scalability: Scalability refers to the ability of a system to handle increasing amounts of workload or data in a reliable and efficient manner. A system is said to be scalable if it can maintain its performance and quality of service as the workload or data volume grows over time. Scalability is an important consideration for many systems, particularly those that are expected to handle large amounts of traffic or data, or that need to support a growing user base.
> 

A major challenge associated with data replication is keeping data replicas consistent

Ali: What does it mean by keeping data consistent? 

Keeping data replicas consistent means ensuring that all copies of the same data have the same values at all times. This is important to prevent conflicts and inconsistencies that can arise when different copies of the data have different values. To maintain consistency, data replication systems often use techniques such as two-phase commit, quorum-based protocols, and conflict detection and resolution mechanisms.

## Increasing Reliability through Replication

If a file system has been replicated, it may be possible to continue working with it after one replica crashes, by simply switching to one of the other replicas.

By maintaining multiple replicas of the same data, it becomes possible to provide better protection against corrupted data.

For example, if we have three copies of the same file for instance, and every write operation is performed on each copy. In this case, we can safeguard ourselves against a single failing write operation, by considering the value that is returned by at least two copies as being the correct one.

## Improving Reliability through Replication

A distributed system needs to scale in terms of size

- Example, when an increasing number of processes needs to access data that are managed by a single server

A distributed system needs to scale in terms of the geographical area it covers:

- Example, by placing a copy of data in proximity of the process using them, the time to access the data decreases. As a consequence, performance, as perceived by that process, increases.
- Note that, although a client process may perceive better performance, it may also be the case that more network bandwidth is consumed to keep all replicas up to date.

For example, if a resident in the UK has to access their twitter account, their requests would be forwarded to the data centre closest to their location, for example in London. Which means their request would be processed quicker than if it were to be processed in a data centre for example, Jakarta.

## The Price of Replication

The problem with replication is that having multiple copies can lead to consistency problems 

Whenever a copy is modified, that copy becomes different from the other copies.

Consequently, modifications have to be carried out on all copies to ensure consistency.

Exactly when and how those modifications need to be carried out determines the price of replication

## Understanding the Price of Replication

Example - Trying to improve access times to Web pages:

To improve performance, web browsers often cache a webpage

<aside>
💡 Cache is a component in a computer system that stores recently accessed data to speed up subsequent accesses. When a program needs to access data from memory, it first checks if the data is present in the cache. If it is, the program can access the data quickly without having to read it from slower memory sources such as a hard disk or remote server. Caching is a commonly used technique for improving performance in computer systems.

</aside>

Pros: Excellent access time from a user’s point of view

Cons: Requires refetching for latest version of a page

### Solution 1

Always fetch pages from the server

- Poor access time if there is no local copy

### Solution 2

Allow web server to invalidate or update each cached copy 

- Degrade the overall performance, as more accesses would be required

## Replication for Performance

Local data replication helps to reduce access time and solve scalability problems

Overall improvement in performance through data replication can be achieved by placing copies of data close to the processes that are using them, which reduces access time and scalability issues are mitigated.

The following problems arise from data replication:

- Keeping multiple copies up to date may require more network bandwidth
- Keeping multiple copies consistent may itself be subject to serious scalability problems (Ali: like what?)

Intuitively, a collection of copies is consistent when the copies are all the same

## Tight Consistency through Synchronous Replication

To guarantee tight consistency, a data update should be performed to all data copies at the same time

In other words, an update must be performed on all copies as a single atomic operation

> Atomic operation: No other operation can be performing when copies are being updated
> 

Implementation of such atomic operation, involving potentially a large number of replicas, is inherently difficult:

- The replicas may be widely dispersed across a large-scale network
- Operations on the replicas may be required to complete quickly

 Difficulties come from the fact that we need to synchronise all replicas

## Relaxing Tight Consistency

Global synchronisation takes a lot of communication time, especially when replicas are spread across a wide-area network

- Solution: Relax the consistency constraints (Ali:  does this mean not expecting data to be consistent? Ahmed: Relax the requirement that updates need to be executed as atomic operation)

The (instantaneous) global synchronisations are avoided if consistency constraints are relaxed, performance may be improved

- Cons: Replicas may not always be the same everywhere

The extent to which consistency can be relaxed highly depends on the following:

- The access and update patterns of the replicated data
- The purpose for which the data is used

A number of consistency models have been considered in a variety of protocols, some of which will be discussed next.

# Data Replication II

## Consistency Models - Assumptions

Assuming that there is a data store to which multiple processes running on different machines have access, a consistency model is a “contract” between the processes and this data store.

- The store promises to work correctly provided that the defined rules are obeyed by the processes.

Local write operations (performed on copy of the data) are propagated to the other copies.

A data operation is classified as write operation when it changes the data, otherwise, is a read operation.

![Untitled](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled.png)

Any consistency model restricts the values that a read operation on a data item can return

## Sequential Consistency Model

A data store is sequentially consistent when it satisfies the following condition:

The result of any execution is the same as if the (read and write) operations by all processes on the data store were executed in some sequential order and the operations of each individual process appear in this sequence in the order specified by it’s program

There is no involvement of time; no reference to the most recent write operation

> Any valid interleaving of read and write operations is acceptable behaviour, but all processes should see the same interleaving of operations
> 

Note:
A process sees the ‘write’ from all processes, but only through it’s own reads.

### Example 1 - Sequential Consistency Model

Consider four processes operating on the same data item $x$

Process $P_1$ first performs a write operation $W_1(x)a$ on $x$ by setting the value of $x$ to $a$.

Later, process $P_2$ also performs a write operation $W_2(x)b$, by setting the value of $x$ to $b$.

Although, both processes $P_3$ and $P_4$ first read the value $b$, and later value $a$, (i.e. the write operation $W_2(x)b$ of process $P_2$ **appears** to have taken place before $W1(x)a$ of $P_1$), the figure shows sequential consistency store.

$P_3$ and $P_4$ reading $b$ first and then $a$ shows consistency with the sequential consistency mechanism

![Untitled](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%201.png)

Ali: I see it as a stack really, I don’t know if that is right. $P_3$ and $P_4$ read the most recently written value for $x$, which is $b$, and then read $a$.

The memory operations from processes $P_1$ and $P_2$ are not ordered with respect to each other, which means that the memory operations from these two processes can be interleaved in any order. Therefore, it is possible that $P_3$ and $P_4$ first read the value b and later read the value $a$, even though the write operation $W_2(x)b$ of process $P_2$ appears to have taken place before $W_1(x)a$ of $P_1$.

<aside>
💡 What is meant by processes not ordered with respect to each other? 
That there is no inherent relationship between them that enforces a specific order. In other words, the memory operations from $P_1$ and $P_2$ can be executed in any order, and the final result should be equivalent to some sequential execution of these memory operations.
In a sequentially consistent memory model, memory operations from different processes must be ordered with respect to each other based on their program order. This means that if two processes have memory operations that access the same data item, the order of those memory operations must be consistent with the order of their execution in the program. However, if there is no inherent relationship between the memory operations from different processes, such as in the case of $P_1$ and $P_2$ in the example, then they are considered to be unordered with respect to each other.

</aside>

### Example 2

The scenario shown below violates sequential consistency because not all processes see the same interleaving of write operations

![Untitled](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%202.png)

$P_3$ will have $a$ as final value of $x$ whereas $P_4$ will conclude that the final value is $b$, which is inconsistent. Also $P_3$ reads $b$ first and $P_4$ reads $a$ first.

## Causal Consistency Model

A data store is causally consistent if it satisfies the condition:

Writes that are potentially causally related must be seen by all processes in the same order. Concurrent writes may be seen in a different order on different machines.

This model distinguishes between events that are possibly causally related and those that are not

- For example, if event $b$ is caused or influenced by an earlier event $a$, causality requires that everyone else first sees $a$, and then $b$.
- Operations that are not causally related are said to be concurrent.

### Example 1

The following event sequence is allowed with causal consistency store only (i.e. it is not a sequentially consistent store)

![Untitled](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%203.png)

$P_2$ and $P_1$  are allowed to write $b$ and $c$ because they are assumed as concurrent. This would not have been allowed in sequential consistency store. (Ali: I do not know the reason why)

Note that the writes $W_2(x)b$ and $W_1(x)c$ are concurrent, so it is not required that all processes see them in the same order.

### Example 2

—This is not causally consistent—

Consider the following event sequence where $W_2(x)b$ potentially depends on $W_1(x)a$ because writing the value $b$ into $x$ may be a result of a computation involving the previously read value by $R_2(x)a$

![Untitled](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%204.png)

Note that the two writes are causally related, so all processes must see them in the same order. Therefore, the event sequence is incorrect

Ali: I do not understand this example again

From Dilan to better understand consistency:
[http://csis.pace.edu/~marchese/CS865/Lectures/Chap7/Chapter7fin.htm](http://csis.pace.edu/~marchese/CS865/Lectures/Chap7/Chapter7fin.htm)

### Example 3

Consider the following event sequence where $W_1(x)a$ and $W_2(x)b$ are concurrent writes

![Untitled](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%205.png)

As a causally consistent store does not require concurrent writes to be globally ordered, this event sequence is correct. Note, however, that it reflects a situation that would not be acceptable for a sequentially consistent store.

### A thought on Causality

![Untitled](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%206.png)

## Replica Management

Replica management is about where, when and by whom replicas should be placed, and subsequently which mechanisms to use for keeping the replicas consistent.

In it’s general form, it is a classical optimisation problem, but in practice, it is often a management/commercial issue

![The nodes represent locations or sites where the replica servers can be placed. The numerical values are the cost factors to access that particular server site](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%207.png)

The nodes represent locations or sites where the replica servers can be placed. The numerical values are the cost factors to access that particular server site

### A Greedy Heuristic to Find Locations

(minimum k-median problem)

1. Find the total cost of accessing each site from all the other sites. Choose the site with the minimum total cost.
2. Repeat (1) above taking also into account sites hosting replicas (i.e. recalculate costs)

**Example**

Using only nodes `E, I, L, M, P` from the graph above:

![Greedy search, finds the quickest possible path](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%208.png)

Greedy search, finds the quickest possible path

Node `M` has the minimum cost, once we choose `M`, the next iteration may take into account the fact that sites attempt to access the nearest replica

**Step 2** (after we choose node `M` for replica hosting):

- Can just remove `M` and repeat to find another node
- Better, but more work, replace each value in the table by min(value, value-to-`M`)
- Note that this destroys any symmetry it had initially

![If row value is greater than M value, change that to M value, else it stays](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%209.png)

If row value is greater than M value, change that to M value, else it stays

![Untitled](Data%20Replication%20e47e4f56916b4574a19eecb4ce2f95e2/Untitled%2010.png)