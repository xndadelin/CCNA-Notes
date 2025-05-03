---
icon: ethernet
---

# Interfaces and Cables

## Interfaces

<figure><img src=".gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

This is a **Cisco Catalyst 2960-L Series 24-Port Switch.**

* It has 24 interfaces, also known as ports (number **4** indication). These are called RJ-45 ports. RJ stands for Registered Jack.&#x20;
* The RJ-45 connector is used on the end of a copper Ethernet cable.

## Ethernet - copper

Ethernet is a collection of network protocols and standards, rather than just a single protocol.

* There are many standards included in the Ethernet.
* Network protocols are important for communication between clients/server. If two services do not agree upon the system of communicating, they cannot talk between them. That’s why there are industry standards that all vendors follow, both in terms of physical standards like connectors and cables like these, as well as logical standards, like IP, the internet protocol.&#x20;
* Connection between devices in a network operate at a set speed. These speeds are measured in bits per second.

### Standards and cables

All the Ethernet standards are defined in the IEEE 802.3 standard. The IEEE is the _Institute of Electrical and electronics engineers._ This standard was defined in 1983.

| Speed    | Common Name      | IEEE Standard                              | Informal Name | Maximum Length |
| -------- | ---------------- | ------------------------------------------ | ------------- | -------------- |
| 10 Mbps  | Ethernet         | <mark style="color:yellow;">802.3i</mark>  | 10BASE-T      | 100 m          |
| 100 Mbps | Fast Ethernet    | <mark style="color:yellow;">802.3u</mark>  | 100BASE-T     | 100 m          |
| 1 Gbps   | Gigabit Ethernet | <mark style="color:yellow;">802.3ab</mark> | 1000BASE-T    | 100 m          |
| 10 Gbps  | 10 Gig Ethernet  | <mark style="color:yellow;">802.3an</mark> | 10GBASE-T     | 100 m          |

**You can learn this table by memorising only "iuaban", 802.3 and to know the speed is going from the 10^1-10^4.**

* **BASE** refers to baseband signaling and **T** refers to twisted pair cabling.
*   The copper cables used in Ethernet standards are UTP cables. UTP stands for:

    * U - UNSHIELDED (no metallic shield, which means there can be electrical interference)
    * T - TWISTED (it's a protection measure against electromagnetic interference - **EMI**)
    * P - PAIR

    <figure><img src=".gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>
* There are 4 pairs in total which means 8 pins. However, not all of the Ethernet standards actually use all 8 wires.
* 10BASE-5 and 100BASE-5, also known as Ethernet and Fast-Ethernet cables, use 2 pairs, or 4 wires. 1000BASE-T and 10GBASE-T, however, use all 4 fours, or 8 wires.

### 10BASE-T and 100BASE-T

Both 10BASE-5 and 100BASE-T standards use 2 pairs of wires, so 4 pins used in total.

<figure><img src=".gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

* As you can see, the **PC** transmits data on pins and <mark style="color:yellow;">**1 and 2**</mark> (Tx), and the **switch** receives data on pins <mark style="color:yellow;">1 and 2</mark> (Rx). Vice-versa, the **switch** sends data on pins <mark style="color:yellow;">**3 and 6**</mark> (Tx) and the **PC** receives data on pins <mark style="color:yellow;">**3 and 6**</mark> (Rx).
* Because the connection between pins do not intersects, there is <mark style="color:yellow;">Full-Duplex</mark> transmission, which means that both devices cand receive and transmit data at the same time.
* Because they transmit and receive on opposite pin pairs, a regular cable like this works well. This kind of cable is called a <mark style="color:yellow;">**straight-through cable**</mark>.&#x20;
* This only works for different network devices, because if I try to connect a router with a router, the pins 1 and 2 transmit data, but the other router cannot receive the data, because the pins 1 and 2 are supposed to send data, not receive data. The router is not prepared to receive data.  This can be fixed using a <mark style="color:yellow;">crossover cable.</mark>
* So, pin 1 on one side connects to Pin 3 on the other side and pin 2 on one side connects to Pin 6 on the other side. As you can see, the two pairs, 1 and 2, and 3 and 6, are reversed. Pin 3 on the left side will connect to Pin 1 on the right side. And pin 6 on the left side will connect to pin 2 on the right side. The wires are ‘crossed over’ each other, hence the name ‘crossover cable’. The transmit pins on one side are connected to the receive pins on the other side, so 17:00 now the two devices can send data to each other with no problems.&#x20;

<figure><img src=".gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

### Auto MDI-X

* Most modern networking devices have evolved beyond having to worry about straight-through cable or crossover cables. That’s because newer networking devices include a feature called Auto MDI-X. Previously, if two switches were connected with a straight-through cable like this, they would be unable to communicate.&#x20;
* However, Auto MDI-X allows devices to automatically detect which pins their neighbor is transmitting data on, and then adjust which pins they use to transmit and receive data.
* So, unless you’re working with network equipment that is quite old, you don’t really have to worry about straight-through and crossover cables.

### 1000BASE-T and 10GB BASE-T

Both 100BASE-5 and 10GBASE-T standards use 4 pairs of wires, so 8 pins used in total.

* The last two pairs are 4 and 5, and 7 and 8.
* In addition to using all four pairs of wires, in 1000BASE-T and 10GBASE-T, each pair is bidirectional, meaning each pair isn’t dedicated specifically to transmitting data or receiving data. This is part of the reason that they can operate at much faster speeds.

## Ethernet - fiber optic&#x20;

The switch whose interfaces I put up the page has 24 ports for RJ45 connectors. These are the ports I would connect a copper UTP cable to. But there are other four interfaces (indicated by number **7**).

* In those 4 interfaces you insert one of these it's called an <mark style="color:yellow;">**SFP**</mark> transceiver. <mark style="color:yellow;">**SFP**</mark> means <mark style="color:yellow;">**S**</mark>mall <mark style="color:yellow;">**F**</mark>orm-Factor <mark style="color:yellow;">**P**</mark>luggable.&#x20;
* Into one of these transceivers you insert a fiber optic cable. Rather than an electrical signal over copper wiring, these cables send light over glass fibers. Notice that there are two connectors on each end. That’s because you need one connector to transmit data, and one to receive data, on each end.
* The copper UTP cables used separate wire pairs within the cable to transmit and receive data. The fiber-optic cables instead use separate cables to transmit and receive.

<div align="center"><figure><img src=".gitbook/assets/image (65).png" alt="" width="375"><figcaption></figcaption></figure></div>

### Structure of the fiber optic cable

<figure><img src=".gitbook/assets/image (66).png" alt="" width="541"><figcaption></figcaption></figure>

* There are four numbered parts in this diagram, from the center to the outer layer.&#x20;
* Number 1 is the fiberglass core itself.  Light is transmitted down this core to transmit data from one device to another.&#x20;
* Number 2 is cladding that reflects light.
* Number 3 is a protective buffer, which protects the fiberglass from breaking.
* Number 4 is the outer jacket of the cable. Now there are a couple main types of fiber-optic cables. single-mode fiber, and multimode fiber.&#x20;

### Types of fiber-optic cables

There are a couple main types of fiber-optic cables. single-mode fiber, and multimode fiber.

#### Multimode fiber

<figure><img src=".gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

* These are two examples of multimode fiber cables. The center represents the fiberglass core, and the blue represents the reflective cladding that reflects light down the cable.&#x20;
* In multimode fiber cable, the core, the actual glass fiber, is wider than single mode fiber. This wider core allows multiple angles, known as modes, of lightwaves to enter the fiber glass core, as you can see in these two diagrams.&#x20;
* Notice the red and black lines representing light waves travelling down the fiberglass core, reflecting off the cladding at different angles.
* Multimode fiber allows longer cables than UTP, which are limited to 100 meters, but still shorter than single-mode fiber.&#x20;
* Multimode fiber cables are cheaper than single-mode fiber, and this is because they use cheaper transmitters, which are often LED-based.

#### Single-mode fiber&#x20;

This is an example of a single-mode fiber cable. The core diameter of a single-mode fiber cable is narrower than a multimode fiber, meaning the glass fiber is thinner. Light enters at a single angle, known as a mode, from a laser-based transmitter. Notice in the diagram that the light wave travels straight down the core of the cable. Single-mode fiber allows longer cable lengths than both UTP and multimode fiber cables. And single-mode fiber cables are more expensive than multimode fiber cables due to the more expensive laser-based transmitters used.

<figure><img src=".gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

### Cable standards for fiber-optic

| Informal Name | IEEE Standard | Speed   | Cable Type               | Maximum Length                 |
| ------------- | ------------- | ------- | ------------------------ | ------------------------------ |
| 1000BASE-LX   | 802.3z        | 1 Gbps  | Multimode or Single-Mode | <p>550 m (MM)<br>5 km (SM)</p> |
| 10GBASE-SR    | 802.3ae       | 10 Gbps | Multimode                | 400 m                          |
| 10GBASE-LR    | 802.3ae       | 10 Gbps | Single-Mode              | 10 km                          |
| 10GBASE-ER    | 802.3ae       | 10 Gbps | Single-Mode              | 30 km                          |

* LX = Long (X = MM or X = SM)
* SR = Short Range
* LR = Long Range
* ER = Extra Long Range

| Feature            | UTP                                                                                  | Fiber-Optic                                                                                 |
| ------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| Cost               | Lower cost than fiber-optic                                                          | Higher cost than UTP                                                                        |
| Maximum Distance   | Shorter (\~100m)                                                                     | Longer than UTP                                                                             |
| EMI Vulnerability  | Can be vulnerable to EMI (Electromagnetic Interference)                              | No vulnerability to EMI                                                                     |
| Port Type and Cost | RJ45 ports are cheaper than SFP ports                                                | SFP ports are more expensive than RJ45 ports (single-mode is more expensive than multimode) |
| Signal Leakage     | Emit (leak) a faint signal outside of the cable, which can be copied (security risk) | Does not emit any signal outside of the cable (no security risk)                            |
