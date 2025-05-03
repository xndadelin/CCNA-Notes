---
icon: truck-arrow-right
---

# TCP & UDP

## Functions of layer 4 protocols

* Provides transparent transfer of data between end hosts.
* Provides (or not provide) various services to applications:
  * Reliable data transfer.
  * Error recovery.
    * That means making sure that the destination host actually received every bit of data that it’s supposed to.
  * Data sequencing.
    * Making sure that even if data arrives at the destination out of order, the end host can sequence it in the correct order.
  * Flow control.
    * One more is flow control, making sure that the source host doesn’t send traffic faster than the destination host can handle. These are services provided by TCP but not UDP.
* Provides Layer 4 addresing (**port** numbers)
  * Note that the word ‘port’ can also refer to the physical interfaces you connect cables to on network devices, but the Layer 4 port is a totally different meaning of the word.
  * These port numbers provide a few functions, one of them is identifying the Application Layer protocol that is being used. Another is to provide something called ‘session multiplexing’.
  * The port numbers that Application Layer protocols use are registered with the _**IANA**_, the Internet Assigned Numbers Authority.
  * They have designated the following ranges.&#x20;
    * Well-known port numbers are ports 0 through 1023. These are used for major protocols like HTTP, FTP, etc, and are very strictly regulated.
    * Registered port numbers are in the range 1024 to 49151.
    * Finally, the range 49152 through 65535 is used for ‘ephemeral’ ports, also known as private or dynamic ports. Hosts use this range when selecting the random source port.

## TCP

* TCP is a connection-oriented protocol.
* Before actually sending data to the destination host, the two hosts communicate to establish a connection. Once the connection is established, the data exchange begins.
* The source host doesn’t just start sending data without first communicating with the destination host and setting up this connection.
* TCP provides reliable communication.
  * The destination host must acknowledge that it received each TCP segment.
  * If the source host doesn’t receive an acknowledgment for a segment, it is sent again.
* TCP provides sequencing.
  * The sequence numbers in the TCP header allow destination hosts to put segments in the correct order even if they arrive out of order.
* TCP provides flow control.
  * That means that the destination host can tell the source host to increase or decrease the rate that data is sent, so that it isn’t overwhelmed by receiving traffic faster than it can process it.
* The TCP header has a minimum size of 20 bytes (when no options are included). Here’s the breakdown:
  * **Source Port:** 2 bytes
  * **Destination Port:** 2 bytes
  * **Sequence Number:** 4 bytes
  * **Acknowledgment Number:** 4 bytes
  * **Data Offset / Reserved / Flags:** 2 bytes
    * The Data Offset field (4 bits) specifies the header length in 32-bit words, which means a 20-byte header corresponds to a value of 5.
    * The Reserved bits and Control flags (totaling 12 bits) are packed with the Data Offset in these 2 bytes.
  * **Window Size:** 2 bytes
  * **Checksum:** 2 bytes
  * **Urgent Pointer:** 2 bytes

### Establishing connections: 3-way handshake

<figure><img src=".gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

### Terminating connections: 4-way handshake

<figure><img src=".gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

### Sequencing / Acknowledgment

<figure><img src=".gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

* Hosts set a random initial sequence number.
* Forward acknowledgement is used to indicate the sequence number of the next segment the host expects to receive.

### Retransmission

<figure><img src=".gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

### Flow control: Window Size

* Acknowledging every single segment, no matter what size, is inefficient.
* However, the TCP header’s window size field allows more data to be sent before an acknowledgment is required.
* In addition, a ‘sliding window’ is used to dynamically adjust how large the window size is.

<figure><img src=".gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

## UDP

* UDP is **not** connection-oriented.
* Unlike TCP, in UDP the sending host does not establish a connection with the destination host before sending data. It is simply sent.&#x20;
* UDP does not provide reliable communication.&#x20;
* When UDP is used, acknowledgments are not sent for received segments. If a segment is lost, UDP has no mechanism to re-transmit it. Segments are sent ‘best-effort’.
* UDP does not provide sequencing. Unlike TCP, UDP has no sequence field in its header. If segments arrive out of order, UDP has no mechanism to put them back in order.
* Finally, UDP does not provide flow control. It has no mechanism like TCP’s window size to control the flow of data.
* The UDP header is fixed at 8 bytes, and it comprises the following fields:
  * **Source Port:** 2 bytes
  * **Destination Port:** 2 bytes
  * **Length:** 2 bytes (specifies the total length of the UDP packet, including header and data)
  * **Checksum:** 2 bytes.

## Comparing TCP & UDP

* TCP provides more features than UDP, but at the cost of additional overhead because of the larger header. In addition, acknowledgments and retransmissions can slow down the transfer of data.
* For applications that require reliable communications, for example downloading a file, TCP is preferred.
* On the other hand, for applications like real-time voice and video, for example voice over IP phone calls, Zoom, Skype, etc, UDP is preferred. These applications are very delay-sensitive, you don’t want the overhead of TCP slowing it down.
* One thing to note is that there are some applications that use UDP, but provide reliability and such within the application itself. TFTP, the Trivial File Transfer Protocol, is such an example.
* Finally, there are some applications that use both TCP & UDP, depending on the situation. DNS, the Domain Name System, is an example.

| TCP              | UDP                | TCP & UDP |
| ---------------- | ------------------ | --------- |
| FTP data (20)    | DHCP server (67)   | DNS (53)  |
| FTP control (21) | DHCP client (68)   |           |
| SSH (22)         | TFTP (69)          |           |
| Telnet (23)      | SNMP agent (161)   |           |
| SMTP (25)        | SNMP manager (162) |           |
| HTTP (80)        | Syslog (514)       |           |
| POP3 (110)       |                    |           |
| HTTPS (443)      |                    |           |

