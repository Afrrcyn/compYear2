# Synchronization

# Synchronization I

Components of a distributed system are autonomous and do not follow a single global clock to help synchronize their tasks

<aside>
🙀 Autonomous: Each component operates independently and has its own decision-making capabilities

</aside>

The term synchronization refers to one of two distinct but related concepts:

1. Data Synchronization
    
    Refers to keeping multiple copies of a dataset in coherence with one another, given that the various copies are located in different nodes
    
    - For example, when you ‘sync’ a mobile device to a laptop to copy photos back and forth, this is a form of data synchronization
2. Process Synchronization
    
    Refers to multiple processes needing to act together to achieve a certain overall purpose
    

Synchronization between processes requires fast and reliable communication between the processes

As in distributed systems, none of these assumptions is safe, synchronization behaviour is often challenging

### When is Synchronization needed?

Two cases again:

1. Multiple processes need to agree on the ordering of events, such as whether message $m_1$ from process $P$ was sent before or after message $m_2$ from process $Q$
    
    Messages in a distributed system need to be ordered to avoid errors. The order in which messages are sent from one process may be different to the order in which the messages arrive at their destination
    
2. Multiple processes try to simultaneously access a shared resource, such as a printer, and should, instead, cooperate in granting each other temporary exclusive access

<aside>
🙀 Accessing a shared resource can result in a deadlock (multiple resources sharing the same resources)

</aside>

## Synchronization Challenges in Distributed Systems

Since nodes in a distributed system are connected via a network, and networks are not always reliable, coordination of actions that depend on communication over a network is quite challenging

- For example, the same message sent to different nodes can take a different time interval to arrive at each node; if sent multiple times to the same node, it may take different times to arrive at the node, or sometimes it may not arrive at all (😩)

Sending a synchronisation message from one node to another could be considered, however because there is no way of knowing exactly how long a message is going to take to arrive at it’s destination, this does not represent a good solution to this problem

Another alternative could be to timestamp a message as it’s sent out, but as there is no way of knowing how long the message took to arrive at it’s destination unless the recipient’s clock is in sync with the sender’s clock in the first place, which cannot be guaranteed as nodes are independent.

In fact, about two nodes with independent clocks, the only thin that is certain is that a message will not arrive at it’s destination before it is sent

### Example: Obtaining the Ordering of Events

![Understand the last bullet point about p5 and why does that happen](Synchronization%200ae07334db4f462bac69e67c7ff0f98a/Untitled.png)

Understand the last bullet point about p5 and why does that happen

Beware of the following:

- If $a$ and $b$ are events in the same process, and $a$ occurs before $b$, then $a\rightarrow b$ (event $a$ happens before $b$) is true

<aside>
🙀 $\rightarrow$ (Happens-before): A transitive relation, so if $a \rightarrow b$ and $b \rightarrow c$, then $a \rightarrow c$. In simpler terms, $a$  would happen before $c$

</aside>

- If $a$ is the event of a message being sent by one process, and $b$ is the event of the message being received by another process, then $a \rightarrow b$ is also true. A message cannot be received before it is sent, or even at the same time it is sent, since it takes a finite, non-zero amount of time to arrive
- If two events, $x$ and $y$ happen in different processes, that do not exchange messages (not even indirectly via third parties), then $x \rightarrow y$ is not true, but neither is $y \rightarrow x$. These events are said to be concurrent, which simply means that nothing can be said (or need to be said) about when the events happened of which event happened first

## Logical Clocks

Logical clocks take advantage of the fact that an implicit partial ordering of events can be obtained from the simple sending and receiving of messages between processes in a distributed system. As such, they do not measure ‘real time’ but, instead provide a distributed incremental pseudo time to events in a distributed system.

A simple example is Lamport’s logical clock, which assumes that each processor $i$ has a logical clock, $LCi$

### Lamport’s Logical Clock

1. When an event occurs on processor $i$, $LCi$ is incremented by one ($LCi = LCI + 1$)
2. When processor $X$ sends a message to Y, it also sends it’s logical clock, $LCx$
3. When $Y$ receives the message, if it’s local logical clock is already in advance of the clock it has just received plus one timestep, it keeps it’s current logical clock, otherwise it sets it’s logical clock to be the one it has just received plus one timestep, i.e.:
    
    $$
    if LCy < (LCx + 1):\\
    LCy = LCx + 1
    $$
    
    ### Notes on Lamport’s Logical Clock
    
    1. The receipt of a message forces the recipient to move it’s clock forward so that the “happened after” relationship is preserved at that point
    2. if $a \rightarrow b$ then it is true that $LCa < LCb$. But it is not necessarily true that just because $LCa < LCb$ then $a \rightarrow b$. This means that we cannot infer a causal relationship just by looking at timestamps.
    3. By using logical clocks, we can obtain a basic partial ordering of events, i.e., we can tell that one event happened after another one, but we do not obtain a perfectly synchronised global time

# Synchronization II

### Example Task: Stopping events from happening simultaneously when sharing resources

![Untitled](Synchronization%200ae07334db4f462bac69e67c7ff0f98a/Untitled%201.png)

## Centralised Lock Server and Mutual Exclusion Locks (Mutex)

Client:

1. Sends a request to the lock server for a mutex on a given resource

1. When a reply comes back, it starts executing it’s critical process over the resource
2. When it’s finished, it sends a message to release the mutex

The Lock Server:

1. If the resource is available, it marks it as being used by the client and sends a reply to say that the lock has been granted. Otherwise, it puts the request in a queue

5. When the mutex is released by the client, if another client wants the resource, (i.e. is in the queue waiting), pass the mutex to them, otherwise mark it as being available

Note:

1. This solution presents a basic limitation: a single point of failure (i.e. the central lock server) - if the central lock server fails, no solution would then work.
2. Although the solution is able to protect sequences of events that need to be treated as ‘atomic’ (indivisible) operations, it cannot make sure that, if any part of the sequence of event fails, the state of the system remains unchanged.

## Two Phase Commit Algorithm

The two phase commit algorithm, ensures that a sequence of events either runs to successful completion or if the sequence fails, that the overall system is returned to it’s original state as though nothing has happened. In other words, it allows intermediate steps that have occurred to be undone (i.e. rollback / backtrack) to return things back to a safe, sensible state, if a fault is detected.

- A coordinator node requests a transaction, and sends a request to all participants nodes
    - e.g. to node $C1$, it sends ‘request to remove $X$ pounds from account’, and to $C2$ sends ‘request to add $X$ pounds to account’
- All participants respond as to whether they are willing and able to execute the request, and send `VOTE_COMMIT` or `VOTE_ABORT`
    - They log their current state, and then perform the transaction
    - All participants log their vote
- The coordinator looks at the votes, if everyone has voted to commit, then the co-ordinator sends a `GLOBAL_COMMIT` to everyone; otherwise it sends a `GLOBAL_ABORT`
- On receiving the decision from the coordinator, all participants record the decision locally. If it was an `ABORT` , participants roll back to their previous safe state

<aside>
🙀 Which node would be chosen as the coordinator?

</aside>

### Electing a Coordinator Node

The Bully Algorithm is a mechanism for choosing a coordinator from a set of candidate nodes. 

The algorithm gets it’s name from the fact that higher numbered nodes ‘bully’ lower numbered nodes into submission

### Bully Algorithm

- $P$ sends an ELECTION message to all nodes with higher numbers
- If no one responds, $P$ wins the election and becomes the coordinator. It informs all the other nodes that it is now the coordinator
- If one of the higher-numbered nodes $Q$ answers, P concedes that it is not the winner, and $Q$ begins the election process again until one node eventually wins.

Note:

1. The algorithm relies on the use of timeouts to decide when to ‘give up’ waiting for responses from nodes that have potentially died (so the usual problem of ‘how long is it reasonable to wait’ applies here
2. The algorithm assumes that the participating nodes are ordered 

## Clock Synchronization

In distributed systems, clock synchronisation is not completely possible by the fact that messages are not sent instantaneously over real networks, and that there usually is some degree of variation in the time (ping?) messages take to arrive at their destination 

As long as some amount of error is acceptable, there are at least two widely acceptable ways for getting clocks in different parts of system into near-synchronisation:

- Cristian’s Algorithm
- The Berkeley Algorithm

## Cristian’s Algorithm

Cristian’s algorithm works between a process $P$, and a time server $S$ connected to a source of UTC (Coordinated Universal Time)

It relies on the accuracy of an estimate based on the Round Trip Time (RTT), which is the elapsed time between the time when a request is sent from $P \space from \space S$

### Algorithm

1. $P$  requests the time from $S$
2. After receiving the request from $P$, $S$ prepares a response and appends the time $T$ from it’s own clock. The response is sent to $P$
3. $P$ then sets it’s time to be 

$$
T + \frac {RTT} {2}
$$

where RTT is Round Trip Time of the request $P$ made to $S$

Note: 

RTT assumes that the RTT is split equally between both request and response, which may not always be the case but is a reasonable assumption on a LAN connection

But what happens if the RTT estimate is not accurate, i.e. it is different when the update is sent to that which was measured? Clocks do tend to drift

And what happens if send and receive do not take the same amount of time, which affects $\frac {RTT} {2}$

These issues are dealt with by a similar but a complex algorithm, The Berkeley Algorithm

## The Berkeley Algorithm

The Berkeley Algorithm does not rely on the assumption that the amount of time that it takes to sent the message is the same as the time it takes to receive the message

Unlike Cristian’s algorithm, the server process in the Berkeley algorithm, called the master, periodically polls other slave processes

### Algorithm

1. A master is chosen via an election process such as the ‘Bully Algorithm’
2. The master polls the slaves who reply with their time in a similar way to ‘Cristian’s Algorithm’
3. The master observes the round-trip time (RTT) of the messages and estimates the time of each slave and it’s own
4. The master then averages the clock times, ignoring any values it receives far outside the values of the others.
5. Instead of sending the updated current time back to the other process, the master then sends out the amount (positive or negative) that each slave must adjust it’s clock. This avoids further uncertainty due to RTT at the slave processes

The clock synchronisation method used in this algorithm, the average cancels out individual clock’s tendencies to drift.

# Deadlock

Deadlock refers to a situation when processes are waiting for one another to finish and so cannot make progress

Deadlock occurs when four particular conditions hold simultaneously in a system:

1. Mutual Exclusion
    
    At least one resource must be non-shareable, i.e. only one process can use that resource at a given time
    
2. Hold and wait
    
    A process is currently holding at least one resource, and is requesting at least one more resource that is currently being held by another process (i.e. it is waiting for a process to release something)
    
3. No pre-emption
    
    Once a process has acquired a resource, nothing (e.g. the operating system or some external factor) can force it to relinquish that resource; it has to do so voluntarily
    
4. Circular wait
    
    A process must be waiting for a resource that is being held by another process, which in turn and possibly indirectly, is waiting for the first process to release a resource
    
    For example, processes are waiting for one another in such a way that there is a cycle of dependencies between them. For example, if A waits for B, B waits for C, and C waits for A, we have a cycle of waiting that would fulfil this condition
    

### Example

Consider a situation where two processes are running at the same time; one of them is using a printer, and the other, a scanner. It comes a point in time when:

- both processes can no longer continue to do their work until they can get access to the device that is being used by the other process
- The processes will not relinquish control of the device they are currently using until they get it

The result is: both processes are in a deadlock state - neither can continue because they are waiting for the other

## Dealing with a Deadlock

Three potentially sensible strategies for dealing with a deadlock: (PAD = padlock = lock)

1. Prevent:
    
    Prevent a deadlock from occurring in the first place by making sure that it is never possible for all four of the conditions to be true at the same time
    
2. Avoid:
    
    Avoid a deadlock by making sure that, even though it is potentially possible for all conditions to hold at the same time, that they never do
    
3. Detect:
    
    Detect a deadlock and deal with the consequences (this last approach is very similar to ‘avoid’, in that it requires having some mechanism for detecting whether all four conditions are about to hold (for avoid) or actually do hold (for detect)
    

Note:

Whether you decide to go for a detect or avoid approach really depends on how likely or frequently a deadlock is likely to happen - if some property of your system means that deadlock is possible, but hugely unlikely, it might make sense to allow it to occur once in a while and to then deal with the consequences, rather than to incur the overhead of trying to avoid it from happening in the first place