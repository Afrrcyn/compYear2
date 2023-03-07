# Introduction to Distributed Systems

# About Distributed Systems

- Complex systems, encompassing not one but a (large) collection of computers, all connected via a network, over which they communicate by message parsing
    
    <aside>
    💡 Message Parsing:
    
    </aside>
    
- Their complexity is very well hidden that users are not aware of the fact that they are dealing with a distributed system

## The Evolution of Computer Systems

From 1945 - 1985

- Computers were large and expensive
- Computers operated independently from one another as it was not possible to connect them

From 1985

- Technology advanced (fr)
    - Development of powerful microprocessors
    - Invention of high-speed computer networks
    - Miniaturization of computer systems

The result of these technologies is that now it is possible and relatively easy to put together a computing system composed of large numbers of networked computers, large or small.

As these computers are generally dispersed, they are said to form a distributed system 🙀

### Examples of Distributed Systems

1. Web over the Internet
    1. Documents, email, media, commerce, etc
2. Mobile telephony
    1. Calls, texts, location, sharing, etc
3. Electronic funds transfer
    1. Banking, credit cards etc
4. Instant messaging
5. Video conferencing
6. Home entertaining systems
7. Global positioning systems

## Benefits of Distributed Systems

1. To share resources:
    - One publisher, many beneficiaries.
    - E.g. google drive, overleaf, e.t.c.
2. To bind customers and suppliers
    - Sending requirements to suppliers so they could come up with a product
3. Performance, scalability, reliability and availability issues
    1. Performance issues: If using 1 computer takes 60 minutes, 100 computers would take 0.6 minutes
    2. Scalability issues: If there’s 10 times more to do in the 0.6 minutes I have, I’ll use 1000 computers, then if things quieten down again, I’ll go back to using 100 computes
    3. Reliability and availability issues: If 1% of computers fail everyday, add an extra 1% to keep the 0.6 stable and on

## Why are Distributed Systems then used?

1. To make continuously-evolving, remote resources accessible for sharing
2. To open proprietary processes to external interaction in order to foster cooperation
3. To achieve better performance / cost ratios
4. To scale effectively and efficiently if demand for resources changes significantly
5. To scale through modular, incremental expansion and contraction
6. To achieve high levels of reliability and availability

## What is a Distributed System?

A distributed system is a collection of autonomous computing elements that appears to it’s users as a single coherent system

Where:

- A computing element or a node, can be either a hardware device or a software process
    - A node can be anything from a high-performance mainframe computer to a small device in a sensor network. Nodes can also be interconnected in anyway
- In a single coherent system, users (i.e. people or applications) believe they are dealing with a single system
    - For this to be possible, the autonomous nodes need to collaborate. The way in which this collaboration is established represents the most fundamental property that distinguishes between different distributed systems.
        
        <aside>
        💡 As a consequence (of autonomous nodes) each node will have it’s own notion of time
        
        </aside>
        

## Characteristics of Distributed Systems

- Distributed systems can also vary in:
    - Size (few to millions of computers)
- The way in which nodes are interconnected (wired, wireless, hybrid (combination of both) networks)
- The way in which node membership is handled (by being open or closed group when allowing new nodes to join)
- Although nodes can act independently from one another (how?), they cannot ignore one another. Otherwise no point in putting them to compose the same system
- Nodes are programmed to achieve common goals, and they do this by exchanging messages with each other
- To appear to users as a single coherent system, distribution transparency is an important goal of distributed systems

In addition: 

- Computation is concurrent
    - There is more than one application running (how do we make sure there is no mishap?)
- There is no shared state
    - There is no single global clock that all programs can agree to follow (how do we make programs synchronize their actions)
- Failures occur, and we may not know or be told about them (how can we tell whether a remote program crashed or is just taking long?)

- Communication events have non-negligible duration
    - Communication costs may become more significant than processing times
- Components may exchange data at variable rates
    - Uncertainty as to when and how long is always present
- Different components process data at different rates
    - Asynchrony (non-aligned timelines) is unavoidable

## Fallacies about Distributed Systems

Distributed systems differ from traditional software because components are dispersed across a network. Not taking this dispersion into account during design time is what makes so many systems needlessly complex and results in flaws that need to be patched later on

Some false assumptions for distributed systems

1. Network is reliable
2. Network is secure
3. Network is homogeneous (of the same kind, uniform)
4. Topology does not change
5. Latency is zero
6. Bandwidth is infinite
7. Transport cost is zero
8. There is one administrator

### Fallacy 1: Network is Reliable

The way in which nodes of a distributed system are linked (a.k.a network topology) may vary. 

A distributed system is composed of different and varied (i.e. heterogenous) resources (software and hardware), which are deployed by different people / organisations.

In essence, the result is a network of networks, each managed with local scope by a different group of administrators

As a consequence, no (sufficiently complex to be useful) network of networks is reliable

Software: Lack of software reliability - need for reliable message exchange between nodes (e.g. functionality for retrying messages, acknowledging messages, verifying message integrity)

Hardware: Hardware associated power failures - network switches have a mean time between failures (e.g. the route between you and the server you get data from). Need for weighting the risks of failure versus the required investment to build redundancy - a trade off (what does this mean?)

### Fallacy 2: Network is Secure

Implications:

- You may need to build security into your applications from day 1
- As a result of security considerations, you might not be able to access networked resources =, different user accounts may have different privileges and so on

### Fallacy 3: Network is Homogenous

A homogenous network today is the exception, not the rule, even a home network may connect a Linux PC and a Windows PC

Implications:

- Interoperability will be needed (what is that?)
- And so, the use of standard technologies such as XML or JSON will be necessary

Homogeneous: of the same kind, uniform

Markup language: Combines text and extra information about the text - designed to facilitate the sharing of data across different information systems, the drawback is that it is very slow

### Fallacy 4: Topology does not change

The topology does not change as long as we stay in the lab

In the wild (bruh) servers may be added and removed often, clients (laptops, wireless ad hoc networks) are coming and going: topology is constantly changing

Implications:

- Do not rely on specific endpoints or routes
- Abstract from the physical structure of the network, by using DNS names as opposed to IP addresses (which may vary) for referring to an endpoint

Network Topology: The way in which nodes of a distributed system are linked
DNS Names: (Internet domain name system) is a hierarchical and decentralized naming system for computers, services, or other resources connected to the internet or a private network. Commonly used to translate more readily memorized domain names to the numerical IP addresses needed for locating and identifying computer services and devices with the underlying network protocols

### Fallacy 5: Latency is Zero

The minimum round-trip between two points on earth is determined by the maximum speed of information transmission: the speed of light

- At 300,000 km/s it will take at least 30msec to send a ping from Europe to the USA and back

Implications:

- Strive to make as few calls over the network as possible (and transfer as much data out in each of these calls)

Network Latency: Time measure of how long it takes for data to move from one place to another

Ping: Computer network administration software utility that measures the round-trip time for messages sent from the originating host to a destination computer that are echoed back to the source

### Fallacy 6: Bandwidth is infinite

Bandwidth trends show that it constantly grows, but so does the amount of information we are trying to squeeze through it (e.g. videos, audios, messages, etc)

To avoid congestion and increase connection throughput, the loss of data packets being transmitted from one point to another can be performed.

- To avoid packet loss, we may want to use larger packet sizes

Implications: 

- Data compression: try to simulate the production environment to get an estimate for your needs

Bandwidth: Measure of how much data it is possible to transfer over a period of time (measured in bits/second)

Throughput: Measure of how much data was successfully transferred from source to destination within a given timeframe (or how many packets arrive at destination successfully). It can be measured in bits/second

### Fallacy 7: Transport cost is Zero

Going from the application layer to the transport later (2nd highest in the five later TCP/IP reference model for network communication) is not free:

- Information needs to be serialised (by marshalling) to get data onto the wire.
- The cost (in terms of money) for setting and running the network is not zero.

Marshalling: Process of transforming the memory representation of an object to a data format suitable for storage or transmission

![Untitled](Introduction%20to%20Distributed%20Systems%20eab4181d97024977adff9d751980c4ac/Untitled.png)

### Fallacy 8: There is One Administrator

There will be different administrators associated with the network with different degrees of expertise

Might make it difficult to locate problems (their problem or our problem)

Coordination of upgrades: Will the new version of MySQL work as before with Ruby on rails

Don’t underestimate the human (social) factor

## Distributed Systems Organisation

To assist the development of distributed applications, distributed systems are often organized to have a separate layer of software that is logically placed on top of the respective operating systems of the computers that are part of the system

![Untitled](Introduction%20to%20Distributed%20Systems%20eab4181d97024977adff9d751980c4ac/Untitled%201.png)

This separate layer is called middle layer and offers each application the same interface as well as the following:

- Facilities for inter-application communication
- Security services
- Support for transaction management
- Recovery from failures
- Support for web services composition

Web services composition: It is becoming increasingly common to develop new applications by taking existing programs and gluing them together

## Designed for Transparency

![Untitled](Introduction%20to%20Distributed%20Systems%20eab4181d97024977adff9d751980c4ac/Untitled%202.png)