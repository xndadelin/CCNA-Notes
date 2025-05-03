---
description: OSI Model & TCP/IP Suite
icon: globe
---

# OSI & TCP/IP

* Networking **models** categorize and provide a structure for networking protocols and standards.
* A networking protocol is a set of (logical) rules defining how network devices and software should work.
* The OSI model is one attempt at standardizing network communications. (not actually in use today, but it has had a big impact on how network engineers think about networking, and we still refer to it today).

<figure><img src=".gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

## OSI Model

* Stands for 'Open Systems Interconnection' model.
* A conceptual model that categorizes and standardizes the different functions in a network.
* Created by the ISO (International Organization for Standardization).
* Functions are divided into 7 'Layers'.
* These layers work together to make the network work.

<table><thead><tr><th width="393">Layer</th><th>Definitions</th></tr></thead><tbody><tr><td>Application</td><td>This layer is closest to the end user. Interacts with software applications. HTTP and HTTPS are Layer 7 protocols. Identifies communication partners, synchronizes communications etc. <strong>It provides process-to-process communication.</strong></td></tr><tr><td>Presentation</td><td>Data in the application layer is in 'application format'. It needs to be 'translated' to a different format to be sent over the network. The Presentation Layer's job is to translate between application and network formats. One example of a function of the presentation layer is encryption of data as it send, so that only the intended recipient can read it, and of course decryption as it is received. The presentation layer also translates between different application-layer formats, to ensure that the data is in a format the receiving host can understand.</td></tr><tr><td>Session</td><td>Controls dialogues (sessions) between communicating hosts. Establishes, manages, and terminates connections between the local application.</td></tr><tr><td>Transport</td><td>Segments and reassembles data for communications between end hosts. Breaks large pieces of data into smaller segments which can be more easily sent over the network and are less likely to cause transmission problems if errors occurs. <strong>Provide host-to-host communication</strong>.</td></tr><tr><td>Network</td><td><strong>Provides connectivity between end hosts</strong> on different networks (outside the LAN). Provides IPs. Provides path selection between source and destination. Layer 3 provides the means of selecting the best path. Routers operate at Layer 3.</td></tr><tr><td>Data Link</td><td><strong>Provides node-to-node connectivity</strong> and data transfer (for example, PC to switch, switch to router, router to router). Because Layer 2 is adjacent to Layer 1, the physical layer, it also defines how data is formatted for transmission over a physical medium, like, copper UTP cables. Detects and (possibly) corrects Physical Layer errors. Uses Layer 2 addressing, separate from Layer 3 addressing. Switches operate at Layer 2.</td></tr><tr><td>Physical</td><td>Defines physical characteristics of the medium used to transfer data between devices (voltage levels, maximum transmission distances, physical connectors, cable specifications) etc. Digital bits are converted intro electrical (for wired connections) or radio (for wireless connection) signals.</td></tr></tbody></table>

<mark style="color:blue;">**Please Do Not Throw Sausage PIzza Away**</mark>**&#x20;- Acronim**

* **Encapsulation** - The data is processed through the OSI stack, from 7 to 1, each layer adding something to the original data. Data is prepared by the top 3 layers, a Layer 4 header is added on. At this point in the process, this unit of data plus the header is called a segment. Next, that segment is passed on to Layer 3.
* The original data is **ENCAPSULATED** inside this additional information which is added on.
* **De-encapsulation** - The data is processed through the OSI stack, from 1 to 7, the additions of the layers are stripped off until the data reaches the application layer of the neighboring system. So basically, the additional information is removed as the data is processed up the stack.
* Both the encapsulation and de-encapsulation processes are examples of '**Adjacent-layer interaction**'. Interaction between the different layers of the OSI model.

<figure><img src=".gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

* The communications between the application layers of the two different systems is called '**same-layer interaction**'. This is what allows the application layer to perform its functions of identifying communication partners, synchronizing communications etc.

<figure><img src=".gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

* Application developers work with the top layers of the OSI model to connect their applications over networks.
* The combination of data, Layer 4 header and Layer 3 header, is called a packet. Next, the packet is further encapsulated at Layer 2, this time with both a Layer 2 header and a Layer 2 Trailer. This is called a frame. This frame is then sent over the connection whether it's electrical signals over a wire or wireless signals in the case of WI-FI, to the neighboring system.

## PDUs

* The term that is used to refer to Data, Segment, Packet, Frame is called PDU (**Protocol Data Units**).
* At Layer 1, the physical layer, the name for the PDU is bit, referring to the bits being transferred on the wire.&#x20;

<figure><img src=".gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

## TCP / IP Suite

* Conceptual model and set of communications protocols used in the Internet and other networks.
* Known as TCP/IP because those are two of the foundational protocols in the suite.
* Developed by the United States Department of Defense through DARPA (Defense Advanced Research Projects Agency).
* Similar structure to the OSI Model, but with fewer layers.
* This is the model actually in use in modern networks.
* NOTE: The OSI model still influences how network engineers think and talk about networks.

| TCP/IP      | OSI                                                       |
| ----------- | --------------------------------------------------------- |
| Application | Application, Presentation and Session from the OSI model. |
| Transport   | The same as the OSI model.                                |
| Internet    | It's the Network Layer of the OSI model.                  |
| Link        | The Data-Link and Physical Layer of the OSI model.        |

* Application, Presentation and Session layers of the OSI model are essentially equivalent to the Application Layer of the TCP/IP model.
* The OSI model and the TCP/IP model both share the transport layer.
* The Network Layer of the OSI model maps to the Internet Layer of the TCP/IP model.
* The Data Link and Physical Layers of the OSI model are equivalent to the Link layer of the TCP/IP model.&#x20;

<figure><img src=".gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>
