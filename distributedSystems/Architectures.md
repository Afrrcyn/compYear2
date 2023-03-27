# Architectures

# Architectures of Distributed Systems I

An obvious way to distinguish between distributed systems is on the organisation of their software components and how they interact. In other words, their software architecture.

Types of architectures:

1. Centralised Architecture: E.g. traditional client-server model, where a single server implements most of the software components (and thus functionality), while remote clients can access that server using simple communication means.
2. Decentralised Architecture: E.g. peer-to-peer architectures in which all nodes more or less play equal roles.
3. Hybrid Architecture: Combining elements from centralized and decentralised architectures, e.g. torrenting (?)

The final instantiation of the software componenets of a distributed system is what is called system architecture

## Software Architectural Styles

A software architectural style is formulated in terms of components, the way that components are connected to each other, data exchanged between componenets, and finally how these elements are jointly configured in a system

> Component: A component is a modular unit with well-defined required and provided interfaces that is replaceable within it’s environment
> 

> Connector: A connector is a mechanism that mediates communication, coordination, or cooperation among components. In other words, a connector allows for the flow of control and data between components
> 

A variety of architectural styles result from the creation of systems configurations using components and connectors, including:

1. Layered Architecture Styles
2. Object-based Architecture Styles
3. Resouce-centered Architecture Styles
4. Event-based Architecture Styles

### Layered Architecture Style

In layered architecture style, components are organized in a layered fashion where a component at layer $L_j$ can make a downcall to a component at a lower-level layer $L_i$ (where i $< j$) and generally expects a response

The layers at the bottom provide a service to the layers on top. The request flows from top to bottom, whereas the response flows from bottom to top. In this approach, the calls always follow a predefined path.

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled.png)

Example: Communication protocol stacks, where each layer implements one or several communication services allowing data to be sent from a destination to one or several targets

### Object-based Architecture Style

In object-based architecture style, each objects corresponds to a component, and these components are connected through a procedure call mechanism, that can take place over a network, if the calling object is not on the same machine as the called object.

Object-based architectures provide a natural way of encapsulating data and the operations that can be performed on that data into a single entity.

Therefore, communication between objects happen as method invocations. These are generally called Remote Procedure Calls (RPC)

Often argued that object-based architectures form the foundation of encapsulating services into independent units. By clearly separating various services such that they can operate independently, the road toward service-oriented architectures (SOAs) is paved

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%201.png)

### Resource-Centered Architecture Style

In resource-centered architecture style, a distributed system is viewed as a huge collection of resources that are individually managed by components. Resources may be added or removed by (remote) applications, and likewise can be retrieved or modified.

It is based on a data center, where the primary communication happens via a central repository.

This approach has been widely adopted for the web.

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%202.png)

### Event-Based Architecture Style

In event-based architecture style, the system will not allow direct communication between it’s nodes, but instead, the nodes will communicate through an events bus. 

Processes running on the various components are both referentially decoupled and temporarily coupled. In other words, one process does not explicitly know any other process, but for coordination to take place processes need to be running at the same time.

In such scenarios, the only thing a process can do is publish a notification (describing the occurrence of an event)

Assuming that notifications come in all sorts and kinds, processes may subscribe to a specific kind of notification. 

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%203.png)

## Types of Architectures

### Centralised System Architecture (Example)

**Simple Client-Server Architecutes**

Processes in a distributed system are divided into two main groups: Clients and servers

A server is a process implementing a specific service, for example, a file system service or a database service

A client is a process that requests a service from a server by sending it a request and, subsequently, waiting for the server’s reply.

The client-server interaction is known as request-reply behavior

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%204.png)

### Multi-Tiered Client-Server Architecture

Typically presents three logical tiers, instead of two that were client and server

In this architecture, the server contains two of the logical tiers, one assoicated with processing and the other associated with the data

The distinction into three logical tiers suggests a number of possibilities for physically distributing a client-server application across several machines. However, the simplest organization is to only have two types of machines:

1. A client machine containing only (part of) the user-interface level
2. A server machine containing the rest, i.e. the programs implementing the processing and data management functionalities

In the most typical organization, all functionality is handled by the server, while the client is essentially no more than a dumb terminal

![The many other possibilities in functionality distribution between client and server machine are illustrated in this figure](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%205.png)

The many other possibilities in functionality distribution between client and server machine are illustrated in this figure

Many distributed applications are divided into the three layers:

- User interface layer,
- Processing layer,
- Data layer.

The main challenge to clients and servers is to distribute these layers across different machines

A server may sometimes need to act as a client, as shown in the figure below, typically leading to a physically three-tiered architecture

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%206.png)

An example of use of this architecture is in the organisation of websites.

Mutli-tiered client-server architectures are a consequence of dividing distributed applications into a user interface, processing and data management components, where the different tiers correspond directly with the logical organization of applications. This type of distribution is called vertical distribution, which is achieved by placing logically different components on different machines.

# Architectures of Distributed Systems II

## Types of Architectures (Continued)

### Decentralised System Architecture (Example)

****************************************Peer-to-Peer Systems****************************************

<aside>
💡 Peer-to-peer System: A peer-to-peer system is a type of decentralised system architecture in which all nodes or participants in the network have equal roles and responsibilities. Each node can act as both a client and a server, and they can communicate and exchange data with each other directly. This type of architecture is often used in file sharing systems, where each participant can share and download files from other participants without the need for a central server.

</aside>

In decentralised architectures, there is a greater concern about distributing client and server functionality more evenly across machines to achieve better workload balance

As a consequence, a client (or server) may be physically split up into a number of logical parts, with each part operating on “it’s own share of the complete data set”. This is known as horizontal distribution of functionality

A class modern system architectures that support this hoirzontal distribution is known as peer-to-peer systems.

From a high-level perspective, the processes that constitute a peer-to-peer system are all equal.

Much of the interaction between processes is symmetric: each process will act as a client and a server

Due to the symmetric behaviour of processes in peer-to-peer architectures, processes are organised in an overlay network;

- What is meant by overlay network? It’s nodes are formed by the processes and it’s links represent the possible communication channels (TCP connections)
    
    > The transmission control protocol (TCP) is one of the main internet protocols
    > 
    
    <aside>
    💡 Transmission control protocol (TCP): Transmission Control Protocol (TCP) is one of the main protocols used on the internet. It is responsible for ensuring that data packets are transmitted reliably from one device to another. TCP is a connection-oriented protocol, which means that it establishes a dedicated connection between devices before sending data. It is used in combination with the Internet Protocol (IP) to form the TCP/IP protocol suite, which is the basis for communication on the internet.
    
    </aside>
    
- Thus node may not be able to communicate directly with an aribitrary other node, but is required to send messages through the available communication channels

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%207.png)

Two types of overlay networks exist, characterising peer-to-peer systems as:

- Structured
- Unstructured

### Structured Peer-to-Peer Systems

In structured P2P systems, nodes are organized in an overlay manner that adheres to a specific, deterministic topology, a ring, a binary tree, a grid, e.t.c.

This topology is used to efficiently look up data that is maintained by the system:

- i.e. each data item is uniquely associated with a key, typically obtained by a hash function on the data item’s value. This key is used as an index, since it identifies a node in the system.
    
    > key (data item) = hash (data item’s value)
    > 

The topology of a structued peer-to-peer system plays a crucial role: any node can be asked to look up a given key, i.e. to efficiently route a request for data to the node responsible for storing the data associated with the given key. 

**Example**

A peer-to-peer system with a fixed number of nodes, organised into a hypercube

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%208.png)

Each data item is associated with one of the 16 nodes of the hypercube - by hashing the value of a data item to a key $k \ \epsilon \ \{1,2,…, 2^4-1\}$

So suppose if we start at node 0111, and we have to lookup key 14, which corresponds to the binary value 1110, which is the end node. To get to the end node, we see there is no direct link to the end node, hence we can choose neighbours of the end node, which in this case are 1111 and 0110. So we can use either of the two nodes to get to the end node. The neighbour nodes are called internodes.

The internodes can then forward the request to the end node 1110, from which the data can be retrieved

### Unstructured Peer to Peer Systems

In an unstructured peer-to-peer system, each node maintains an ad hoc list of neighbours, such that the resulting overlay resembles a random graph: a graph in which an edge $<u,v>$ between two nodes $u$ and $v$ exists only with a certain possibility $P[<u,v>]$. Ideally, this probability is the same for all pairs of nodes.

<aside>
💡 Ad hoc means "for this specific purpose" or "created or done for a particular purpose as necessary". In an unstructured peer-to-peer system, each node maintains an ad hoc list of neighbours, meaning that it is a list of neighboring nodes that is created or done for the specific purpose of that node's participation in the system. The list is not predetermined or fixed, but rather created as necessary for the node's function within the system.

</aside>

When a node joins in, it often contacts a well-known node to obtain a starting list of other peers in the system. This list can then be used to find other more peers, and perhaps ignore others, and so on. In practice, a node generally changes it’s local list almost continuously

Unlike structured P2P systems, looking up data cannot follow a predetermined route, because lists of neighbours are constructed in an ad hoc fashion. Instead, searching for data is necessary.

### Searching Methods

**Flooding**

Assume an issuing node, $u$, passes a request for a data item to all it’s neighbours

Each of it’s neighbours, v:

- Ignores the request when it has seen before, otherwise, searches locally for the requested data item
- If it has the required data, it can either respond directly to the issuing node $u$ or send it back to the original forwarder, who will then return it to it’s original forwarder and so on. If it does not have the requested data, it forwards the request to all it’s neighbours.

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%209.png)

Ali: Looking at this searching algorithm, we can see that this algorithm will be computationally expensive, also would take a really long time to search for the data (would it really?)

Flooding is expensive, for which reason a request often has an associated *time-to-live* or TTL value giving the maximum number of hops a request is allowed to be forwarded.

Choosing the right TTL value is important. A small TTL value means that the request will stay close to the issuer and may thus not reach the node having the data (goal node). However, too large of a TTL value would incur communication costs.

************Random Walks************

An issuing node $**u**$, tries to find a data item by asking a randomly chosen neighbour, $v$.

If $v$ does not have the data, it forwards the request to one of it’s randomly chosen neighbours, and so on.

Generally, a random walk imposes less network traffic than flooding, but it may take longer before a node is reached that has the requested data. To decrease the waiting time, an issuer can simply start $n$ random walks simultaneously.

A random walk also needs to be stopped. To end this, a TTL value can be used, or alternatively, when a node receives a lookup request, it can check with the issuer whether forwarding the request to another randomly selected neighbour is still needed.

> Notably in unstructured P2P systems, locating relevant data items can become problematic as the network grows, causing a scalability problem.
> 

### Making Data Search more Scalable in Unstructured P2P Systems

To improve scalability of data search, P2P systems can make use of special nodes that maintain an index of data items, abandoning their symmetric nature by creating special collaborations among nodes.

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%2010.png)

For example, in a collaborative content delivery network (CDN), nodes may offer storage for hosting copies of web documents, allowing web clients to access pages nearby, and thus to access them quickly. 

## Hybrid Architectures

Encompasses classes of distributed systems in which client-server solutions are combined with decentralised architectures. Example: 

- Edge-server systems
- Collaborative distributed systems

One of the main motivations for these hybrid architectures, is the scalability problems of unstructured peer-to-peer systems, and the difficulties in workload balancing in traditional client-server architectures.

### Edge-server systems

Edge-server systems are characterised by the following main properties:

- Deployed on the internet
- Their servers are placed “at the edge” of the network (i.e. the boundary between enterprise networks and the actual internet)

For example, an Internet Service Provider is considered to be an edge server system (?)

The edge server’s main purpose is to serve content, possibly after applying filtering and transcoding functions.

- For a specific organisation, one edge server acts as an origin server from which all content originates. That server can use other edge servers for replicating high-demand content, e.g. Web pages

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%2011.png)

Edge server systems have recently been used to assist data centers in cloud computations and storage, leading to distributed cloud systems. In the case of fog computing, even end-user devices form part of the system and are (partly) controlled by a cloud-service provider.

### Collaborative Distributed Systems

Combine traditional client-server structures (when nodes are joining the system) and fully decentralised structures (once a node has joined the system). An example of such system is BitTorrent which is a peer-to-peer file downloading system.

In BitTorrent, an end user, looking for a file, downloads chunks of the file from other users, until the downloaded chunks can be assembled together, yielding the complete file.

**How does BitTorrent work?**

- To download a file, a user needs to access a global directory containing references to torrent files.
- A torrent file contains the information that is needed to download a specific file, such as a link to a file tracker, a server that keeps an accurate account of active nodes that have (chunks of) the requested file.
- Once the nodes have been identified from where chunks can be downloaded, the downloading node effectively becomes active.

> There will be many different trackers, although there will generally be only a single tracker per file (or collection of files). Once the nodes have been identified from where chunks can be downloaded, the downloaded node effectively becomes active.
> 

![Untitled](Architectures%20822c3b6b43c44dc6bf12520f6e737fff/Untitled%2012.png)