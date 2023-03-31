# Service Oriented Architecture

# Service Oriented Architecture Style

This architecture style encapsulates services into independent units (we do not need to know how the services are implemented, we only need to know what they do)

A service is considered as a discrete unit of functionality that can be access remotely, via a network, and updated independently. However a service can possibly make use of other services.

- Example: A procedure to retrieve a credit card statement online

Distributed systems or distributed applications constructed using this architecture style are said to be having a Service-Oriented Architecture (SOA).

A distributed system constructed as SOA is an essentially a composition of many different services.

## Example of a SOA Application

Consider a distributed system application composed of several services for processing e-book orders from a web shop

Note that not all services composing of a distributed system application may belong to the same administrative organisation

![Untitled](Service%20Oriented%20Architecture%20ebccabe3cf1941969e288928d2394110/Untitled.png)

## Service Composition

One of the main challenges of developing a distributed system is one of service composition and making sure that the services operate in harmony

For service composition to be possible, each service must offer an interface (including the allowed input and output messages)

Service composition is far from trivial problem

One of the potential issues with service composition is that various components can turn into an integration nightmare when combining the services