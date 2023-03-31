# Security

# Security I

Security in distributed systems can roughly be divided into two parts: 

1. Communication between users or processes, possibly residing on different machines
2. Authorisation, to ensure that a process gets only those access rights to the resources in a distributed system it is entitled to.

The main method for dealing with security in communication (1) is that of secure channel

Methods for dealing with authorisation (2) are called access control mechanisms

## Relationship between Security and Dependability

Security in a distributed system is associated with the notion of dependability, because a dependable system is one that is trusted to deliver it’s services

Recall that dependability is about availability, reliability, safety and maintainability

- Which indirectly suggests trust (also includes confidentiality and integrity)
1. Confidentiality: Refers to the property of a computer system whereby it’s information is disclosed only to authorised parties
2. Integrity: The characteristic that alterations to a system’s assets can be made only in an authorised way. Also the process of ensuring and preserving the validity and accuracy of data  to it’s end users

Users of the distributed system should not receive corrupted data

## Types of Security Threats

Security in computer systems involves mechanisms to protect system services and data against security threats

There are four types of security threats:

1. Interception: An unauthorised party has gained access to a service or data. 
Example: When communication between two parties has been overheard by someone else
2. Interruption: Services or data become unavailable, unusable, destroyed, and so on.
Example: Denial of service attacks by which someone maliciously attempts to make a service inaccessible to other parties; when a file is corrupted or lost, e.t.c
3. Modification: Unauthorised changing of data or tampering with a service
Example: Intercepting and subsequently changing transmitted data, tampering with database entries, e.t.c.
4. Fabrication: Additional data or activity are generated that would normally not exist.
Example: an intruder may attempt to add an entry into a password file or database; breaking into a system by replaying previously sent messages

## Security Mechanisms

A description of actions that are allowed to be taken and actions that are prohibited to guarantee system security is what we call Security Policy which impacts users, services, data, machines, e.t.c.

Based on system’s security policy, mechanisms can be put in place to enforce this policy:

- Encryption
- Authentication
- Authorisation
- Auditing

### Encryption

Encryption transforms data into something an attacker cannot understand

As such, encryption is not only able to implement data confidentiality, but it also supports data integrity checks, because it allows checking of whether data has been modified.

A key is generally used to encrypt data using a mathematical algorithm.

There are two types of encryption: Symmetric and Asymmetric encryption

1. In symmetric encryption the same secret value (key) is used for encryption and decryption whereas different keys are used in asymmetric encryption
    
    ![Untitled](Security%202a67ef762c854cc393c1d7d3107b91be/Untitled.png)
    
    A safe method of communication should be used to send they key to the receiver from sender, as whoever gets the key can easily decrypt the message
    
    ---
    
2. In asymmetric encryption, two keys are used. The key with the sender is the public key, which is a global / shared key. The key with the receiver is a private key which only the receiver has. Authentication
    
    ![Untitled](Security%202a67ef762c854cc393c1d7d3107b91be/Untitled%201.png)
    
    ---
    

### Authentication

Authentication is used to verify the claimed identity of a user, client, server, host, or other entity.

Authentication is based on the possession of some secret information, like password, known only to the entities participating in the authentication.

When an entity wants to authenticate another entity, the former will verify if the latter possesses the knowledge of the secret. 

Authentication can be one-way or mutual

- In one-way authentication, only one entity verifies the identity of the other entity.
- In mutual authentication, both communicating entities verify each other’s identity.

### Authorising and Auditing

After a user has authenticated, we perhaps need to provide access rights to them, which is called authorisation

Authorisation checks whether a client is authorised to perform the action requested

- Example: For accessing records in a medical database, such that depending on who accesses the database, permission may be granted to read records, to modify certain fields in a record.

Auditing tools are used to trace which clients accessed what, and in which way.

Audit logs can be extremely useful for the analysis of a security breach, and subsequently taking measures against intruders.

Attackers often do not want to leave any traces behind, which is why auditing is a good tool in recording traces of activities.

# Security II

## Cryptography

Cryptography is a fundamental security technique in distributed systems

Consider a sender S wanting to transmit message $m$ to a receiver $R$

To protect the message against security threats, the sender:

1. Encrypts it to an unintelligible message $m'$
2. Sends $m'$ to $R$
3. $R$, in turn, must decrypt the received message into it’s original form $m$

![Untitled](Security%202a67ef762c854cc393c1d7d3107b91be/Untitled%202.png)

Note that the original form of the message that is sent is called the plaintext, shown as $P$; the encrypted form is referred to as the cipher-text, illustrated as $C$

## Security in Distributed System (Recap)

Two main issues that need to be addressed:

1. Making the communication between clients and servers secure
    - This may involve: authentication of the communicating parties as well as integrity and confidentiality of messages
2. Controlling access to resources
    - Once a server has accepted a request from a client, how can it find out whether that client has authorised to have that request carried out?

### Secure Channels

The issue of protecting communication between clients and servers, can be thought of in terms of setting up a secure channel between communicating parties

- A secure channel protects senders and receivers against interception, modification, and fabrication of messages
- Protecting messages against interception is done by ensuring confidentiality: the secure channel ensures that it’s messages cannot be eavesdropped by intruders
- Protecting against modification and fabrication by intruders is done through protocols for mutual authentication and message integrity

**Example of Authentication Protocol**

Consider than Alice and Bob want to communicate, and that Alice takes the initiative in setting up a channel

Alice starts by sending a message to Bob, or otherwise to a trusted third party who will help set up the channel

Once the channel has been set up, Alice knows for sure that she is talking to Bob, and Bob knows for sure he is talking to Alice, they can exchange messages

![Authentication based on a shared secret key](Security%202a67ef762c854cc393c1d7d3107b91be/Untitled%203.png)

Authentication based on a shared secret key

Note: 

Updating an existing protocol to improve it’s performance can easily effect it’s correctness as well.

Besides authentication, a secure channel should also provide guarantees for message integrity and confidentiality 

> Message integrity: Ensures that messages are protected against illegal modification
> 

Confidentiality is established by simply encrypting a message before sending it

Message integrity can be obtained via the use of digital signatures

## Digital Signatures

A digital signature is a mathematical technique that is used to validate the authenticity and integrity of a message, not a software or digital document.

Consider the situation where Bob has just sold Alice a collector’s item of some vinyl record for £500. The whole deal was done through email.

Besides authentication, at least two concerns that need to be addressed: 

1. Alice needs to be assured that Bob will not maliciously change the £500 and claim she promised more than £500
2. Bob needs to be assured that Alice cannot deny ever having sent the message

Possible situation: Alice digitally signs the message, uniquely binding her signature to it’s content

- The unique association between a message and it’s signature prevents illegitimate modifications and backing out from the agreement

One common form of digital signature is to use a public-key crypto-system

![Untitled](Security%202a67ef762c854cc393c1d7d3107b91be/Untitled%204.png)

When Alice sends a message to Bob, she encrypts it with her private key $K_A^-$, and sends it off to Bob. If he also wants to keep the message content a secret, she can use Bob’s public key and send the encrypted message, which combines $m$ and the value signed by Alice. When the message arrives at Bob, he can decrypt it using Alice’s public key, if he can be assured that the public key is indeed owned by Alice, then decrypting the signed version of $m$ and successfully completing it to $m$ can only mean that it has come from Alice.

Alice is protected against any malicious modifications to $m$ by Bob, because Bob will always have to prove that the modified version of $m$ was also signed by Alice. In simpler terms, the decrypted message alone never counts as proof.

## Controlling Access to Resources

![Untitled](Security%202a67ef762c854cc393c1d7d3107b91be/Untitled%205.png)

Controlling the access to an object means protecting it against requests generated by unauthorised subjects

Protection is often enforced by a program called a reference monitor

> A reference monitor records which subject may do what, and decides whether a subject is allowed to have a specific operation carried out
> 

Reference monitor should be impenetrable by it’s very nature

Reference monitor will see if the subject that is requesting the access for a particular object has the right access rules in place. If it has the access rights available enabled, only then will it authorise the request.