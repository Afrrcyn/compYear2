# Inter-Process Communication

Goal: Understanding how processes running on different nodes communicate in a distributed system

# Inter-Process Communication I

Inter-process communication refers to the exchange of information between processes running on different machines of a distributed system.

Inter-process communication encompasses the ways that processes on different machines can exchange information

Traditional inter-process communication has always been based on low-level message passing, as offered by the underlying network; thus it it harder to realise than communication based on shared memory, as available for non-distributed platform.

As modern distributed systems often consist of thousands or even millions of processes scattered across a network with unreliable communication such as the internet, development of large-scale distributed applications is very different.

Two widely-used models for communication are:

1. Remote Procedure Calls (RPC)
2. Message-Oriented Middleware (MOM)

## Remote Procedure Calls (RPC)

Communication transparency cannot be achieved with traditional inter-process communication, as low-level operations (send and receive) do not conceal communication

Based on the idea that programs can call procedures located on other machines, an RPC aims at hiding most of the intricacies (details) of message passing, and is ideal for client-server applications

For example, when a process on machine A calls a procedure on machine B, the calling process on A is suspended, and execution of the called procedure takes place on B. Information can then be transported from the caller to the callee in the parameters and can come back in the procedure result. This way, no message passing is visible to the programmer.

### Detailed Example

A program has access to a database that allows it to append data to a stored list, after which it returns a reference to the modified list. The operation is made available to a program by means of routine append: 

`newlist = append(data, dblist)`

When append is a remote procedure, a different version of append (a.k.a client stub) is offered to the calling client. The client stub packs the parameters into a message and requests that message to be sent to the server, by calling send. Following the call to send, the client stub calls receive, blocking itself until the reply comes back. 

<aside>
💡 Stub: In general, a stub is a small piece of code that stands in for a larger program. It can be used for testing purposes, or as a placeholder until the full program is available.

</aside>

![Untitled](Inter-Process%20Communication%206a21169171c94581aa8ec905113be2b3/Untitled.png)

The idea behind RPC is to make a remote procedure call look as much possible like a local one

When the message arrives at the server, the server’s OS passes it to a server stub. The server stub unpacks the parameters from the message and then calls the server procedure in the usual way. The server performs it’s work and then returns the result to the caller (the server stub)

> Server stub: Server-side equivalent of a client-stub: it is a piece of code that transforms requests coming in over the network into local procedure calls
> 

### Summary

1. The client procedure calls the client stub in the normal way
2. The client stub builds a message and calls the local operating system.
3. The client’s OS sends the message to the remote OS
4. The remote OS gives the message to the server stub
5. The server stub unpacks the parameters and calls the server
6. The server does the work and returns the result to the stub
7. The server stub packs the result in a message and calls it’s local OS
8. The server’s OS sends the message to the client’s OS
9. The client’s OS gives the message to the client stub
10. The stub then unpacks the result and returns it to the client.

Further explanation of the steps:

In the context of remote procedure call (RPC), a stub is a piece of code that acts as a proxy between the client and server components. It allows the client to invoke a procedure on the server as if it were a local call.

A client stub is a piece of code that resides on the client side and is responsible for marshalling (gathering) the parameters of the procedure call into a message that can be transmitted over the network to the server. The client stub also receives the response from the server, unpacks it and returns the result to the client program.

A server stub is a piece of code that resides on the server side and is responsible for receiving the incoming message from the client, unpacking the parameters, invoking the actual server-side procedure with the arguments, and packing the result in a message that can be sent back to the client.

With that in mind, let's go through the steps in the summary you provided:

1. The client program calls the client stub as if it were calling a local procedure.
2. The client stub marshals the procedure arguments into a message, which is then sent to the local operating system.
3. The client's operating system sends the message over the network to the remote operating system.
4. The remote operating system receives the message and passes it on to the server stub.
5. The server stub unpacks the message and calls the actual server-side procedure with the provided arguments.
6. The server-side procedure does its work and returns the result to the server stub.
7. The server stub marshals the result into a message and sends it to its local operating system.
8. The server's operating system sends the message over the network to the client's operating system.
9. The client's operating system receives the message and passes it on to the client stub.
10. The client stub unpacks the result from the message and returns it to the client program.

That's essentially how remote procedure calls work. The client program can invoke procedures on a remote server as if they were local procedures, and the server can send the results back to the client using a similar mechanism.

# Inter-Process Communication II

## Parameter Passing in RPCs

The function of the client stub is to take it’s parameters, pack them into a message, and send them to the server stub.

Packing parameters into a message is called Parameter Marshalling

<aside>
💡 Marshalling, in the context of remote procedure calls (RPCs), refers to the process of taking parameters passed to a procedure call, and packaging them into a message that can be transmitted over the network to the server. It involves converting the data structures used by the client program into a format that can be transmitted over the network, and then reconstructing those data structures on the server side. This process is also sometimes referred to as serialization or encoding.

</aside>

Parameter Marshalling is not as straightforward:

The server just sees a series of bytes coming, which constitute the original message sent by the client, i.e. no additional information on how those bytes should be interpreted is provided with the message.

And if it were, then how should this meta-information be recognized as such by the server?

![Untitled](Inter-Process%20Communication%206a21169171c94581aa8ec905113be2b3/Untitled%201.png)

The solution to this problem is to transform data into a machine and network-independent format, making sure that both communicating parties expect the same message data type to be transmitted.

- The latter can typically be solved at the level of programming languages.
- The former is accomplished by using machine-dependent routines that transform data to and from machine and network-independent formats

Marshalling is all about this transformation to neutral formats and forms, an essential part of remote procedure calls

![Untitled](Inter-Process%20Communication%206a21169171c94581aa8ec905113be2b3/Untitled%202.png)

## Passing References to Objects in RPCs

How are pointers or (in general) references passed?

By copying the entire data structure to which the parameter is referring, effectively replacing the copy-by-reference mechanism by copy-by-value / restore

![Untitled](Inter-Process%20Communication%206a21169171c94581aa8ec905113be2b3/Untitled%203.png)

Optimisation: When the client stub knows that the refer data will only be read, it also knows that there is no need to copy the structure back when the call has finished.

## Asynchronous RPCs

Calling process suspends execution while waiting for the results from an RPC.

To support situations in which there is simply no result to return to the client, RPC systems may provide facilities to what are called asynchronous RPCs

With asynchronous RPCs, the server immediately sends a reply back to the client the moment the RPC request is received, after which it locally calls the requested procedure. The reply acts as an acknowledgment to the client that the server is going to process the RPC. The client will continue without further blocking as soon as it has received the server’s acknowledgment 

![Untitled](Inter-Process%20Communication%206a21169171c94581aa8ec905113be2b3/Untitled%204.png)

## Message-Oriented Middleware (MOM)

MOM relies on message-queuing mechanisms for providing extensive support for persistent asynchronous communication. As such, message-queuing systems offer intermediate-term storage capacity for messages, without requiring either the sender or receiver to be active during message transmission.

![Untitled](Inter-Process%20Communication%206a21169171c94581aa8ec905113be2b3/Untitled%205.png)

Message queuing systems are typically targeted to support message transfers that are allowed to take minutes instead of seconds or milliseconds.

> The sender posts a message in the queue, and the receiver retrieves the message from the queue
> 

Applications communicate by inserting messages in specific queues, which are then forwarded until eventually delivered to the destination.

A sender is generally given only the guarantees that it’s message will eventually be inserted in the recipient’s queue.

No guarantees are given that when or even if the messages would be read, which is dependent on the behaviour of the recipient

Message-queuing systems are often used to assist the integration of dispersed collections of databases as well as in published-subscribe systems.

Message queues enable asynchronous communication, which means that the endpoints that are producing and consuming messages interact with the queue, not each other. 

Producers can add requests to the queue without waiting for them to be processed. 

Consumers process messages only when they are available.

No component in the system is ever stalled waiting for another, optimizing data flow.