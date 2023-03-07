# Basic Concepts

# Naming Schemes

Naming is important in distributed systems because it is the way by which entities and locations in a potentially large system can be uniquely identified and referred to by the processes running on different nodes of the system

Naming is supported by a naming system, which is no less complex than a distributed system because it is also distributed.

## Naming in Distributed Systems

In distributed systems, names are used to uniquely identify entities, to refer to locations and more

An important issue with naming is that a name can be resolved to the entity it refers to; name resolution thus refers to the means by which a process is allowed to access a named entity, which is supported by a naming system.

<aside>
🙀 Entity: Any device connected to the networks that link the components of a distributed system

</aside>

> Name resolution is the means by which a running process on a node of a distributed system is able to access an entity
Name resolution is also a functionality of a naming system, which is implemented across multiple machines, and so it is also distributed
> 

The implementation of a naming system is itself often distributed across multiple machines.

## Internet Naming

As in any distributed system, every computer connected to the internet needs to be ‘addressable’, so that other computers on the internet are able to ‘talk’ to each other. Naming entities is the addressing mechanism via which a computer on the internet is uniquely identified

Possible approaches to addressing mechanisms include:

1. Centralised
2. Free-for-all
3. By delegating naming responsibilities

> The internet is the biggest distributed system of all being a huge network of networks; any device can connect to it and leave it at any point in time.
> 

### Centralised Naming Approach

Centralised naming is the most obvious approach to guaranteeing that any name is handed out once and only once. In this approach, there is a single point of contact, that either validates a name is unique, or alternatively makes up a unique name and hands that out on demand.

It’s main limitation is that the single point of contact has to deal with every request, and as a consequence, it is not a very scalable solution, and it creates a single point of failure.

> If the central system breaks, then everything stops working
> 

It is as if there is only a single list of names so it is easy to check where the name that is about to be given away to a new device has been given before to another device still connected to the network

### Free-for-All Naming Approach

Free-for-all allows any object that wants a name to make up it’s own name. Although this is a massively ‘distributed’ solution which avoids a single point of failure, it does not guarantee uniqueness.

This approach might not be the ideal solution in a large system as more than one device can coincidentally or not choose the same name. This approach however solves the problem of the centralised naming approach problem by being scalable and massively distributed incurring no single point of failure. It can also not guarantee uniqueness in the way device is connected to the system are named.

### The ‘Delegating Naming Responsibilities’ Approach

In this approach, the authority to allocate names is delegated to smaller parts of the system, and governed by some rules.

This approach better balances the conflicting issues associated with single points of failure and scalability, but it raises questions as to what rules are appropriate for each system.

For example, a rule can be that every device in a particular organisation has to have a name that includes the name of the organisation; another rule could state that every device in a particular  country has to have a name that includes the name of the country

This approach is a hybrid of the two approaches (Centralised and free-for-all). Combines the advantages of having a centralized naming system in one small part of the distributed system. And the advantage of having multiple of these centralized systems associated with different parts of the distributed system so that if one of these fails there will be another one to do it’s job as a consequence. This is thus a more distributed approach without being massively distributed. 

This approach is also the most widely used approach for naming

## Delegation Naming Mechanisms

### MAC Address

Media Access Control (MAC) address is a unique identifier given to each network device in a system: this means that every ethernet or WIFI card in a computer has one MAC address

Note that since most computers have several network devices, there are more MAC addresses than there are computers. A MAC address is a 48 bit number, meaning there are about 281474976710656 different numbers, enough for every person living on Earth to have lots of different network devices

A MAC consists of two parts: 

1. Organisationally Unique Identifier (OUI)
2. Network Interface Controller (NIC)

![Untitled](Basic%20Concepts%2043a7f91080bb48ad8256b396703a1843/Untitled.png)

> A MAC address does NOT tell you where a device is on a network
> 

The OUI is a 24 bit number that is purchased from the Institute of Electrical and Electronics Engineers (IEEE), which acts as a central authority from which vendors of hardware can purchase unique identifiers. Once a vendor has an OUI to use in the MAC addresses for it’s hardware, as long as that particular vendor makes sure that the NIC part of the address is unique, then the combination of the OUI and NIC will be unique

### IP Addresses

An Internet Protocol address (IP Address) serves two purposes: it is a unique identifier (for a device connected to the internet) and also contains some information about where a device is on a network

Most IP addresses are 32 bit numbers, but are most often written as four 8 bit numbers, separated by dots (e.g. 130.88.192.9)

![Untitled](Basic%20Concepts%2043a7f91080bb48ad8256b396703a1843/Untitled%201.png)

There are not enough IP addresses for each person on Earth, and so Ipv6W, the latest generation of IP address, uses 128 bit numbers to overcome this problem.

The top-level authority for IP addresses is the Internet Assigned Numbers Authority (IANA), which delegates ranges of the address space to five regional internet registries, which in turn delegate sub ranges of their space to Internet Service Providers (ISP)

Unlike MAC addresses, the delegation of IP addresses takes place initially to geographical regions (rather than hardware vendors)

> This is why IP addresses can tell you some information about the location of a device on the internet
> 

### Domain Names

Doman names were created because humans find IP addresses hard to read

The Domain Name System (DNS) is itself a Distributed System built on top of the internet, used to create associations between human-readable names and IP addresses.

- For example, [www.bbc.co.uk](http://www.bbc.co.uk) will be translated into an IP address that you can use to communicate with the appropriate computer

The delegation model used by the DNS is complex, as it has aspects of geographical delegation

- For example, domain names ending in `.co.uk` are for UK-based companies
- But it also separates matters out in other ways as well
    - For e.g. domains ending in `.com` and `.org` have no geographical implications but refer to companies and organisations

Unlike MAC and IP addresses, DNS cannot allocate batches of names upfront, and needs to respond in real-time to requests to translate names into IP addresses

It achievers this by being in itself a Distributed System, consisting of a hierarchy of servers with the most authoritative server at the top of the hierarchy, dealing with requests from the users. 

<aside>
🙀 When a server fails it’s job, it’s job is given to the server up in the hierarchy

</aside>

![Untitled](Basic%20Concepts%2043a7f91080bb48ad8256b396703a1843/Untitled%202.png)

# Protocols

Protocols define sets of rules governing how two or more objects should interact with one another. Protocols serve as specifications rather than implementation of a piece of technology

## 1. HTTP

An example of a relevant protocol is the Hypertext Transport Protocol (HTTP) which provides a specification (vocabulary) that allows client applications to request resources from Web servers, and web servers to respond to these requests

![Untitled](Basic%20Concepts%2043a7f91080bb48ad8256b396703a1843/Untitled%203.png)

Some common terminologies used by client applications and Web servers when communicating via the HTTP protocol over the internet:

| HTTP verbs | Description |
| --- | --- |
| HEAD | Asks for the response identical to the one that would correspond to a GET request, but without the response body. This is useful for retrieving meta-information written in response headers, without having to transport the entire content |
| GET | Requests a representation of the specified resource. Requests using GET should only retrieve data and should have no other effect |
| POST | Submits data to be processed (e.g. an HTML form) to the identified resource. The data is included in the body of the request. This may result in the creation of a new resource of the updates of existing resources or both. |
| OPTIONS | Returns a list of the commands supported by this particular server |
| DELETE | Used to delete a resource. It may return the representation of the removed resource |

For more HTTP verbs, www.w3.org/Protocols/rfc2616/rfc2616-sec9.html

### Statelessness

By following the HTTP protocol, web servers are said to be stateless, meaning that once a request from a client application is fulfilled, the web server disconnects from the client and ‘forgets’ that the client ever connected.

The stateless nature of the communication between client and server allows the system to treat each request for content as an independent transaction that can be completed. No state information between requests is preserved

However, there are ways in which state information can be preserved. These are outside of the scope of (this course too) the protocol, but encoded in applications and servers that use it.

Note:

When a web server remembers a client, you should be aware that this functionality was implemented outside of the HTTP protocol.

## 2. Email

Note: Email is not a protocol but uses POP3 and SMTP which are protocols of an emailing service. 

Electronic mail (email) seems like a simpler system than ‘the web’ in many ways it is more complex 🤷

Each client interacts with an **SMTP** server to send out an email, SMTP is an outgoing mail server. Each mail also interacts with a **POP3** server to pick up incoming emails. Hence the POP3 server is an incoming mail server and POP3 is a protocol for receiving emails.

In the diagram to the right, we can see that Alice wants to send an email to Bob, she uses SMTP to send an email to the SMTP server which directs the email to Bob’s POP3 server. It depends on when Bob connects to the server, only then does POP3 pick up the email send by Alice. So the POP3 server waits for Bob to connect first.

![Standard components involved in an email exchange](Basic%20Concepts%2043a7f91080bb48ad8256b396703a1843/Untitled%204.png)

Standard components involved in an email exchange

The expectation is that an email has to be in exactly one place at any one time. 

- If it is in two places, at the same time, then it has been duplicated accidentally.
- If it is in zero places, then it has been lost ‘in the system’ in detriment of the sender and the recipient of that email.

### SMTP (Outgoing)

Simple Mail Transfer Protocol, responsible for sending emails

Like HTTP, SMTP is a text-based protocol. But unlike HTTP, it is connection based, which means that a client (the mail user agent) can issue consecutive comments to the SMTP server, and should explicitly terminate it’s connection when finished

 An example of a typical email exchange (part 1):

![Untitled](Basic%20Concepts%2043a7f91080bb48ad8256b396703a1843/Untitled%205.png)

At the end of this exchange, Bob’s email has successfully been moved from his mail client to his ‘outgoing mail server’

![Part 2](Basic%20Concepts%2043a7f91080bb48ad8256b396703a1843/Untitled%206.png)

Part 2

There are other more recent email protocols too:

1. POP3 (incoming)
2. IMAP