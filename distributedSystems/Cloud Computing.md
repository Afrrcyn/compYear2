# Cloud Computing I

## Drivers of Modern Distributed Systems

### Pervasive Networking Technology

<aside>
💡 Pervasive means "widely spread or prevalent", referring to something that is present or noticeable in every part of something.

</aside>

As a consequence of recent advances in networking and the emergence of the modern internet, networking has become a pervasive resource and devices can be connected (if desired) at any time at any place

Examples of such advances include a wide range of wireless communication technologies, such as Wifi, WiMax, Bluetooth, and mobile phone networks

The internet too is a type of distributed system, that can enable users to share files, send emails, communicate e.t.c

### Ubiquitous Computing (Supporting user mobility)

<aside>
💡 Ubiquitous means "existing or being everywhere at the same time; constantly encountered". In the context of computing, it refers to the ability of devices to be connected and able to access data and services from anywhere, at any time. This is often facilitated by wireless technologies and the internet. Ubiquitous computing supports user mobility, allowing users to access data and services on the go, from a variety of devices.

</aside>

Ubiquitous computing is the harnessing of many small, cheap computational devices that are present in our physical environments and that can communicate with one another.

Examples of users’ physical environments: home, office, natural settings, e.t.c.

Ubiquitous computing can be used, for example, to allow users to control their washing machine or their entertainment system from their phone; to enable a washing machine to notify the user via a phone when the washing is done.

Examples of devices include: Laptop computers, handheld devices (e.g., mobile phones, smart phones, GPS-enabled devices, pagers, personal digital assistants (PDAs), video cameras and digital cameras), wearable devices (e.g., smart watches with functionality similar to a PDA), devices embedded in appliances (e.g., washing machines, hi-fi systems, cars and refrigerators).

Ubiquitous computing supports user mobility because it allows a user to remain connected to an environment as the user moves about.

User mobility is possible because of the high portability of many of these devices, together with their ability to connect conveniently to networks in different places

![Untitled](Cloud%20Computing%20de2be7058ea44fce95590242d4fdd040/Untitled.png)

### Multimedia Services

Distributed systems have been enabled with functionality to support a multitude of multimedia services.

This support is often associated with a range of media types in an integrated manner, often involving storage, transmission and presentation of not only discrete media types (e.g. pictures or text messages), but also continuous media types (e.g. audio and video)

Examples of multimedia services provided within distributed systems include: access to live or pre-recorded television broadcasts, access to film libraries offering video-on-demand services, access to music libraries, the provision of audio and video conferencing facilities and integrated telephony features including IP telephony or related technologies such as Skype, a peer-to-peer alternative to IP telephony.

### Utility Computing

It is a service provisioning paradigm where distributed resources are viewed as a commodity or utility, drawing the analogy between distributed resources and other utilities such as water or electricity.

- Different service suppliers provide resources not owned by the end user.
- This model applies to both physical resources (e.g. storage and processing) and “logical” resources (e.g. email, distributed calendars, CRM software applications, e.t.c.)

# Cloud Computing II

The term cloud computing is used to capture this vision of computing as a utility, where a cloud is defined as a set of internet-based application, storage and computing services to support users’ needs.

![Untitled](Cloud%20Computing%20de2be7058ea44fce95590242d4fdd040/Untitled%201.png)

Cloud computing promotes everything as a service, from physical or virtual infrastructure through to software, often paid for on a per-usage basis rather than purchased.

- As such, it reduces requirements on users’ devices, allowing very simple devices to access a potentially wide range of resources and services.

It has had tremendous impact on both industry and academia, becoming closely embedded in our everyday computing environment, to the extent of becoming invisible to us.

Examples of systems and applications which are Cloud-based that are a part of our everyday life include the following:

- Systems such as Microsoft Office 365, Dropbox, Google Drive (amongst many others) are considered a vital part of our computing infrastructure
- Social media platforms such as Facebook, Twitter and Instagram all make use of Cloud systems as part of their implementation.
- The majority of mobile apps make use of cloud computing

Early scientific Cloud systems, such as Eucalyptus, Open Cirrus, etc., once considered the domain of computer science research, are now regularly used within a variety of other communities, from biological sciences to arts and humanities.

Internet of things (IoT) and Big Data are closely related to cloud computing, as many applications that generate or process large data sizes make use of cloud-based infrastructure, and many IoT devices act as data generators or consumers.

Cloud computing has been widely deployed and become a major backbone of every other technology - from cellular phones through to wearables, connected vehicles, and the future networked society.

## Networked Society

A network society creates an ecosystem of device vendors, application developers, network operators, telecom operators, and cloud services/infrastructure providers to create a foreseeable new business value chain that will not only accelerate every area but also bring new innovative ideas and services.

Cloud technologies became a major part of networked society

![Untitled](Cloud%20Computing%20de2be7058ea44fce95590242d4fdd040/Untitled%202.png)

## Cluster Computers in Cloud Computing

Clouds are generally implemented on cluster computers to provide the necessary scale and performance required by such services.

A cluster computer is a set of interconnected computers that cooperate closely to provide a single, integrated high performance computing capability.

The overall goal of cluster computers is to provide a range of cloud services, including high-performance computing capabilities, mass storage (e.g. through data centres), and richer application services such as web search.

- Google for example, relies on a massive cluster computer architecture to implement it’s search engine and other services.

## Cloud-Computing vs Grid-Computing

The development of the grid preceded the emergence of cloud computing and was a significant factor in it’s emergence.

Cloud and grid computing share the same goal of providing resources (services) out there in the greater internet.

- Grid computing largely focuses on high-end data-heavy or computationally expensive applications
- Cloud computation is more general, offering a range of services for individual computers users through to high-end users.
- The business model associated with cloud computing is also a distinguishing characteristic

It is therefore fair to say that the grid is an early example of cloud computing, but cloud computing has developed significantly since then.

## Virtualisation

Virtualisation is a process that allows more efficient utilisation of physical hardware and it is the foundation of cloud computing

In the same way that resources of a single computer are virtualised by traditional operating systems, a cloud computing layer can virtualise multiple computers

Virtualisation uses a software to create an abstraction layer over computer hardware that allows the hardware elements of using a computer as using more than one computers (to be divided into multiple virtual computers)

Each VM runs it’s own operating system and behaves as an independent computer

Virtualisation enables more efficient utilisation of physical computer hardware

![Untitled](Cloud%20Computing%20de2be7058ea44fce95590242d4fdd040/Untitled%203.png)

In cloud computing, virtualisation enables cloud providers to serve users with their existing physical computer hardware. It enables cloud users to purchase only the computing services that they need.

Virtualisation brings several benefits to cloud service providers, such as resource efficiency, as virtualisation enables maximum utilisation to physical hardwares

Another benefit is easier management 

Third benefit is minimal downtime. If there’s been a server crash or a critical fault, administrators can bring those services up by switching to a virtual machine

## Amazon Web Services

With the view of everything of everything as a service, web services offer a natural implementation path for cloud computing, and indeed many vendors go down this path, one of them being Amazon Web Services (AWS)

Amazon Web Services are a set of cloud services implemented on the extensive physical infrastructure owned by amazon.com

The implementation of AWS takes care of key distributed systems issues such as managing service availability, scalability and performance, allowing developers to focus on the use of their services

More generally, the web services based approach enables interoperability across the internet

### Main AWS Services

![Untitled](Cloud%20Computing%20de2be7058ea44fce95590242d4fdd040/Untitled%204.png)