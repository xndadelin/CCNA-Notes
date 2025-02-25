---
icon: bomb
description: Dynamic routing protocol
---

# OSPF

* Stands for **Open Shortest Path First.**
* Uses the Shortest Path First algorithm of Dutch computer scientist Edsger Dijkstra.
* It has 3 versions:
  * OSPFv1 (1989): OLD, not in use anymore.
  * OSPFv2 (1998): Used for IPv4.
  * OSPFv3 (2008): Used for IPv6 (can also be used for IPv4, but usually v2 is used.
* Routers store information about the network in LSAs (Link State Advertisements), which are organized in a structure called the LSDB (Link State Database).
* Routers will **flood** LSAs until all routers in the OSPF _area_ develop the same map of the network (LSDB).

## LSA Flooding

<figure><img src=".gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

* OSPF is enabled on R4's G1/0 interface.
* R4 creates an LSA to tell its neighbors about the network on G1/0.
* The LSA is flooded throughout the network until all routers have received it.
* This results in all routers sharing the same LSDB.
* Each router then uses the SPF algorithm to calculate its best route to 192.168.4.0/24.
* Each LSA has an aging timer (30 min by default). The LSA will be flooded again after the time expires.

## Process of OSPF

* In OSPF, there are 3 main steps in the process of sharing LSAs and determining the best route to each destination in the network.
  * Become neighbors with other routers connected to the same segment.
  * Exchange LSAs with neighbor routers.
  * Calculate the best routes to each destination, and insert them into the routing table.

## OSPF Areas

* OSPF uses areas to divide up the network.
* Small networks can be _single-area_ without any negative effects on perfomance.
* In larger network areas, a single-area design can have negative effects:
  * The SPF algoritm takes more time to calculate routes and requires exponentially more processing power on the routers
  * The larger LSDB takes up more memory on the routers.
  * Any small change in the network causes every router to flood LSAs and run the SPF algorithm again.
* By dividing a large OSPF network into several smaller areas, you can avoid the above negative effects.

<figure><img src=".gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

* An area is a set of routers and links that share the same LSDB.
* The backbone area (area 0) is an area that all other areas must connect to.
* Routers with all interfaces in the same area are called **internal routers.**
* Routers with interfaces in multiple areas are called **area border routers** (ABRs).
* ABRs maintain a separate LSDB for each area they are connected to. It is recommended that you connect an ABR to a maximum of 2 areas. Connecting an ABR to 3+ areas can overburden the router.
* Routers connected to the backbone area (area 0) are called **backbone routers**.
* An **intra-area route** is a route to a destination inside the same OSPF area.&#x20;
* An **inter-area route** is a route to a destination in a different OSPF area.

### Rules

* OSPF areas should be _contiguous._
  * Each individual area should be connected, not divided up.
* All OSPF areas must have at least one ABR connected to the backbone area.
* OSPF interfaces in the same subnet must be in the same area.

## Basic OSPF Configuration

<figure><img src=".gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

```
R1(config)#router ospf ?
<1-65535>  Process ID

R1(config)#router ospf 1
R1(config-router)#network 10.0.12.0 0.0.0.3
% Incomplete command.

R1(config-router)#network 10.0.12.0 0.0.0.3 area 0
R1(config-router)#network 10.0.13.0 0.0.0.3 area 0
R1(config-router)#network 172.16.1.0 0.0.0.15 area 0
R1(config-router)#
```

* The OSPF process ID is locally significant. Routers with different process IDs can become OSPF neighbors.
* The OSPF network command requires you to specify the area.
* The network command tells OSPF to:
  * Look for any interfaces with an IP address contained in the range specified in the network command.
  * Activate OSPF on the interface in the specified area.
  * The router will then try to become OSPF neighbors with other OSPF-activated neighbor routers.

```
R1(config-router)#passive-interface g2/0
```

* The passive-interface command tells the router to stop sending OSPF 'hello' messages out of the interface.
* However, the router will continue to send LSAs informing it's neighbors about the subnet configured on the interface.
* Should always use this command on interfaces which do not have any OSPF neighbors.

### Advertise a default route in OSPF

```
R1(config-router)#default-information originate 
```

### Configure router-id

```
R1(config-router)#router-id <A.B.C.D>
R1(config-router)#reload
or
R1(config-router)#clear ip ospf process
```

***

An autonomous system boundary router (ASBR) is an OSPF router that connects the OSPF network to an externel network.

R1 is connected to the Internet. By using the default-information originate command, R1 becomes an ASBR.

## OSPF Cost

* OSPF's metric is called cost.
* It is automatically calculated based on the bandwidth (speed) of the interface.
* It is calculated by dividing a **reference bandwidth** value by the interface's bandwidth.
* The default reference bandwidth is 100 Mbps.
* All values than 1 will be converted to 1.
* Therefore Fa, Gig, 10Gig, etc. are equal and all have a cost of 1 by default.
* You can (and should!) change the reference bandwidth with this command

```
R1(config-router)# auto-cost reference-bandwidth megabits-per-second
```

* You should configure a reference bandwidth greater than the fastest links in your network (to allow for future upgrades).
* You should configure the same reference bandwidth on all OSPF routers in the network.
* The OSPF cost to a destination is the total cost of the 'outgoing/exit interfaces'.
* Loopback interfaces have a cost of 1.

```
R1(config-if)ip ospf cost <1-65535>
```

* One more option to change the OSPF cost of an interface is to change the bandwidth of the interface with the bandwidth command.
* Although the bandwidth matches the interface speed by default, changing the interface bandwidth does not actually change the speed at which the interface operates.
* To change the speed at which the interface operates, use the **speed** command.
* Because the bandwidth value is used in other calculations, not just the OSPF cost, it is not recommended to change this value to alter the interface’s OSPF cost.
* It is recommended that you change the reference bandwidth, and then use the ip ospf cost command to change the cost of individual interfaces
* However, if you want to change the bandwidth of the interface, here is the command.

```
R1(config-if)#bandwidth <1-10000000kilobits>
```

```
R3#show ip ospf interface brief
Interface    PID   Area        IP Address/Mask     Cost   State Nbrs F/C
Lo0          1     0           3.3.3.3/32          1      LOOP  0/0
Gi1/0        1     0           192.168.3.126/25    100    DR    0/0
Fa2/0        1     0           10.0.34.1/30        1000   BDR   1/1
Gi0/0        1     0           10.0.13.2/30        100    DR    1/1
```

## OSPF Neighbors

* Making sure that routers successfully become OSPF neighbors is the main task in configuring and troubleshooting OSPF.&#x20;
* Once routers become neighbors, they automatically do the work of sharing network information, calculating routes, etc. So, you just have to make sure that OSPF is activated on the correct interfaces, and that the proper conditions are met to allow the routers to become neighbors.
* When OSPF is activated on an interface, the router starts sending OSPF hello messages out of the interface at regular intervals (determined by the hello timer). These are used to introduce the router to potential OSPF neighbors.
* By exchanging hello messages they check that they are compatible to become OSPF neighbors, and then negotiate their neighbor relationship.
* The default hello timer is 10 seconds on an Ethernet connection.
* OSPF hello messages are multicast to the IP address 224.0.0.5, which is the multicast address for all OSPF routers.
* OSPF messages are encapsulated in an IP header, and the ‘protocol’ field of the IP header has a value of 89 to indicate OSPF.
* For OSPF routers to become neighbors they have to go through various neighbor states.

<figure><img src=".gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

* Let’s assume OSPF is already activated on R2’s G0/0 interface.
* Then, OSPF is activated on R1’s G0/0 interface.&#x20;
* It sends an OSPF hello message to 224.0.0.5.

<figure><img src=".gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

### Down State

* There are more fields in the hello message, but two important ones are R1’s router ID and the neighbor’s router ID. However, R1 doesn’t know about R2 yet, so the neighbor router ID field is 0.0.0.0.
* R1 doesn’t know about any OSPF neighbors yet, so the current neighbor state is Down. This is the first OSPF neighbor state, ‘Down’.

### Init State

* When R2 receives the Hello packet, it will add an entry for R1 to its OSPF neighbor table.
* In R2’s neighbor table, the relationship with R1 is now in the Init state.
* Note that R1 still doesn’t know about R2, so it will have no entries in its OSPF neighbor table.
* Basically, the Init state means that a Hello packet was received, but R2’s own router ID is not in the Hello packet
* R2’s router ID is 2.2.2.2, but the neighbor router ID in R1’s Hello packet is 0.0.0.0.

### 2-way State

* R2 will send a Hello packet containing the RID of both routers. R1 will insert R2 into its OSPF neighbor table in the 2-way state.
* Then, R1 will send another Hello message, this time containing R2’s RID. Now both routers are in the 2-way state.
* The 2-way state means the router has received a Hello packet with its own RID in it.
* If both routers reach the 2-way state, it means that all of the conditions have been met for them to become OSPF neighbors.
* They are now ready to share LSAs to build a common LSDB.
* On the other hand, if they fail to reach this 2-way state, you know that you have to troubleshoot and find what’s stopping them from reaching it.
* In some network types, a DR (Designated Router) and BDR (Backup Designated Router) will be elected at this point.
* At this point, the routers are already OSPF neighbors. Over the next few neighbor states they will share LSAs and form a full OSPF adjacency.

### Exstart State

*   After the 2-way state, the two routers will now prepare to exchange information about

    their LSDB.
* Before that, they have to choose which one will start the exchange.
* So, they will decide which one will be the Master router and which will be the Slave router.
* This Master/Slave relationship is only needed for this initial exchange of LSDB information.
* The router with the higher RID will become the Master and initiate the exchange.
* The router with the lower RID will become the Slave. So, in this case R2 will be the master and R1 will be the slave.
* To decide the Master and Slave, they exchange DBD (Database Description) packets.
* Basically, the Exstart state is just to prepare for the next state.
* R1 sends a DBD packet claiming to be the master. However R2 corrects R1. R2 has the higher router ID, and says that it will be the master.

### Exchange State

* In the next state, the Exchange state, the routers exchange DBDs which contain a list of the LSAs in their LSDB.&#x20;
* These DBDs do not include detailed information about the LSAs, just basic information telling their neighbor what LSAs they have.
* Basically the routers are telling each other, ‘I have these LSAs’, but they aren’t actually sending the LSAs yet.
* The routers compare the information in the DBD they received to the information in their own LSDB to determine which LSAs they must receive from their neighbor.
* After exchanging DBDs, they move to the next state.

### Loading State

* In the Loading state, routers send Link State Request (LSR) messages to request that their neighbors send them any LSAs they don’t have.
* In the Exchange state they exchanged DBD packets, so they know which LSAs their neighbors are holding. So, these LSRs are used to request any missing LSAs to make sure each router has the same LSAs.
* Then the LSAs themselves are sent in Link State Update, LSU, messages. R2 sends R1 the requested LSAs in an LSU like this. R1 will also do the same for R2. Finally, The routers send LSAck messages, another kind of OSPF message, to acknowledge that they received the LSAs. Now the loading state is complete, and the routers have the same LSDB.

<figure><img src=".gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

### Full state

* In the Full state, the routers have a full OSPF adjacency and identical LSDBs.
* But that doesn’t mean things are complete. They continue to send and listen for Hello packets, once every 10 seconds by default, to maintain the neighbor adjacency. To maintain the adjacency another timer called the ‘Dead’ timer is used.
* Every time a Hello packet is received, the ‘Dead’ timer, which is 40 seconds by default, is reset.
* However, if the Dead timer counts down to 0 and no Hello message is received, the neighbor is removed.
* If the neighbors remain up, the routers will continue to share LSAs as the network changes to make sure each router has a complete and accurate map of the network.
* This is the main advantage of dynamic routing protocols, the routers automatically react to changes in the network and add, remove, or change routes as necessary.

<figure><img src=".gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

| Type | Name                               | Purpose                                                                                  |
| ---- | ---------------------------------- | ---------------------------------------------------------------------------------------- |
| 1    | Hello                              | Neighbor discovery and maintenance.                                                      |
| 2    | Database Description (DBD)         | Summary of the LSDB of the router. Used to check if the LSDB of each router is the same. |
| 3    | Link-State Request (LSR)           | Requests specific LSAs from the neighbor.                                                |
| 4    | Link-State Update (LSU)            | Sends specific LSAs to the neighbor.                                                     |
| 5    | Link-State Acknowledgement (LSAck) | Used to acknowledge that the router received a message.                                  |

```
R1#show ip ospf neighbor
Neighbor ID    Pri   State       Dead Time   Address      Interface
3.3.3.3        1     FULL/DR     00:00:39    10.0.13.2    GigabitEthernet1/0
2.2.2.2        1     FULL/DR     00:00:31    10.0.12.2    GigabitEthernet0/0
```

## OSPF Configuration

```
R1(config)#int g0/0
R1(config-if)#ip ospf 1 area 0
R1(config-if)#int g1/0
R1(config-if)#ip ospf 1 area 0
R1(config-if)#int g2/0
R1(config-if)#ip ospf 1 area 0
R1(config-if)#int l0
R1(config-if)#ip ospf 1 area 0
```

• You can activate OSPF directly on an interface with this command:&#x20;

`R1(config-if)#ip ospf process-id area area`

```
R1(config-if)#router ospf 1
R1(config-router)#passive-interface default
R1(config-router)#no passive-interface g0/0
R1(config-router)#no passive-interface g1/0
```

* Configure ALL interfaces as OSPF passive interfaces:&#x20;

`R1(config-router)#passive-interface default`

* Then configure specific interfaces as active:&#x20;

`R1(config-router)#no passive-interface interface-id`

## Loopback interfaces

* A loopback interface is a virtual interface in the router.
* It is always up/up (unless you manually shut it down).
* So, this means that the status of a loopback interface isn’t dependent on a physical interface.
* Physical interfaces can have hardware problems and fail, however that can’t happen to a loopback interface unless the router itself fails.
* So, it provides a consistent IP address that can be used to reach and identify the router. Sometimes you need to send traffic directly to a router.
* Let’s says R1 has no loopback interface at the moment, and R4 receives a packet destined for R1 at 10.0.13.1, the IP address of its G1/0 interface. It might forward it to R1 like this.
* What if R1’s G1/0 interface goes down for some reason? If R4 receives a packet destined for R1 at 10.0.13.1, it will not be able to send the packet to R1.
* How about if R1 has a loopback interface, 1.1.1.1, and that is used to identify R1 instead of 10.0.13.1?
* Even if a physical interface fails, when R4 receives a packet destined for R1’s loopback interface it will still be able to send the packet to R1.
* So, that’s why it’s a good idea to configure a loopback interface on a router. It provides an interface with an IP address that is always up, and can consistently be used to identify and reach the router.

## OSPF Network Types

* The OSPF network type refers to the type of connection between OSPF neighbors, and the type influences how OSPF behaves in some ways. The most common type of connection in modern networks is Ethernet, of course.
* There are three main OSPF network types:
  * The first is the Broadcast network type, which is enabled by default on Ethernet and FDDI interfaces. FDDI is an old technology.
  * The next is the Point-to-point network type, which is enabled by default on PPP and HDLC interfaces.
  * The last main network type is Non-broadcast. It is enabled by default on Frame Relay and X.25 interfaces.

### Broadcast Network Type

<figure><img src=".gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

* This network type is enabled on Ethernet and FDDI interfaces by default.
* In the network diagram above, these are all Ethernet connections, and therefore these connections between the routers are all using the Broadcast network type.
* Routers dynamically discover neighbors by sending and listening for OSPF Hello messages using the multicast address 224.0.0.5. However, not all network types dynamically discover neighbors like this.
* With the ‘Non-broadcast’ network type you must manually configure neighbors.
* A DR, designated router, and BDR, backup designated router, must be elected on each subnet.
* However in cases like the G1/0 interface of R1, R3, R4, and R5 where there are no OSPF neighbors, there is just a DR, no BDR.
* Routers which aren’t the DR or BDR for the subnet become a ‘DROther’.
* Each subnet needs a DR. These subnets are easy, there are no OSPF neighbors so each router becomes the DR for the subnet.

<figure><img src=".gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

* How about the 192.168.1.0/30 subnet between R1 and R2?
* Let’s say R2 is the DR.
* So, R1 becomes the BDR for the segment.&#x20;
* How about the 192.168.2.0/29 subnet that R2, R3, R4, and R5 connect to? For example, R5 might be the DR, R4 the BDR, and then R2 and R3 become DROthers.

#### DR/BDR election

<figure><img src=".gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

* There is an order of priority.
  * First up, the router with the highest OSPF interface priority in the subnet becomes the DR for the segment.
  * However, all interfaces have the same priority by default, so then the routers compare their OSPF router IDs. The router with the highest OSPF router ID wins.
* First place’ in the election becomes the DR for the subnet, and ‘second place’ becomes the BDR.
* The default OSPF interface priority is 1 on all interfaces.
* The command to change the OSPF priority of an interface is:

```
R2(config-if)#ip ospf priority <1-255>
```

* If you set the OSPF interface priority to 0, the router CANNOT be the DR/BDR for the subnet, no matter what.
* The DR/BDR election is ‘non-preemptive’.
* 'non-preemptive’ means is that once the DR/BDR are selected they will keep their role until OSPF is reset, the interface fails/is shut down, etc.
* When the DR goes down, the BDR becomes the new DR. Then an election is held for the next BDR.
* In the broadcast network type, routers will only form a full OSPF adjacency with the DR and BDR of the segment.
* Therefore, routers only exchange LSAs with the DR and BDR. DROthers will not exchange LSAs with each other.
* All routers will still have the same LSDB, but this reduces the amount of LSAs flooding the network.
* When routers need to send messages to the DR and BDR they use multicast address 224.0.0.6.
* The DR and BDR form a full adjacency with ALL routers in the subnet, including the DROthers.
* DROthers will form a FULL adjacency only with the DR/BDR.

### Point-to-Point Network Type

<figure><img src=".gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

* This network type is enabled on serial interfaces using the PPP or HDLC encapsulations by default.
* PPP and HDLC are both Layer 2 encapsulations, similar to Ethernet, except they are used on serial connections.
* Same as the Broadcast network type, routers dynamically discover neighbors by sending/listening for OSPF Hello messages using multicast address 224.0.0.5. However, here’s a difference. A DR and BDR are NOT elected.
* As the network type name implies, these encapsulations are used for ‘point-to-point’ connections between two routers.
* Therefore there is no point in electing a DR and BDR. The two routers will form a Full adjacency with each other, without the need to elect a DR and BDR.

#### Serial Interfaces

<figure><img src=".gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

```
R1(config)#interface s2/0
R1(config-if)#clock rate ?
  With the exception of the following standard values not subject to rounding,

       1200 2400 4800 9600 14400 19200 28800 38400
       56000 64000 128000 2015232

  accepted clockrates will be bestfitted (rounded) to the nearest value
  supportable by the hardware.

  <246-8064000>    DCE clock rate (bits per second)

R1(config-if)#clock rate 64000
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no shut
```

* One side of a serial connection functions as DCE, which stands for Data Communications Equipment.
* The other side functions as DTE, which stands for Data Terminal Equipment.
* On serial connections, the DCE side needs to specify the clock rate, which is the speed, of the connection.
* In this case R1 has the DCE side of the cable and therefore needs to tell R2 what speed the connection will operate at.
* Ethernet interfaces use the SPEED command to configure the interface’s operating speed. Serial interfaces use the CLOCK RATE command.
* On Cisco routers, the default encapsulation on a serial interface is HDLC. Actually, it’s Cisco’s own version called ‘cHDLC’, Cisco HDLC, but it displays as just ‘HDLC’ in the CLI.
* Note that if you change the encapsulation, it must match on both ends or the interface will go down.&#x20;

### Configure the OSPF Network Type

```
R1(config-if)#ip ospf network ?
 broadcast            Specify OSPF broadcast multi-access network
 non-broadcast        Specify OSPF NBMA network
 point-to-multipoint  Specify OSPF point-to-multipoint network
 point-to-point       Specify OSPF point-to-point network
```

* Note that not all network types work on all link types. For example, a serial link cannot use the broadcast network type. This is because serial links don’t support Layer 2 broadcast frames, which is necessary for the broadcast network type.

| Broadcast                                | Point-to-point                               |
| ---------------------------------------- | -------------------------------------------- |
| Default on **Ethernet, FDDI** interfaces | Default on **HDLC, PPP** (serial) interfaces |
| DR/DBR elected                           | No DR/BDR                                    |
| Neighbors dynamically discovered         | Neighbors dynamically discovered             |
| Default timers: Hello 10, Dead 40        | Default timers: Hello 10, Dead 40            |

The non-broadcast network type uses a default Hello timer of 30 seconds and Dead timer of 120 seconds.

## Neighbors Requirements

<figure><img src=".gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

1. Area number must match.
2. Interfaces must be in the same subnet.
3. OSPF process must be not **shutdown.**
   1. `R2(config-router)#shutdown`
4. OSPF router-id must be unique.
5. Hello and Dead timers must match.
   1. `R2(config-if0#ip ospf hello-interval/dead-interval <seconds>`
6. Authentication settings must match

```
R2(config-if)#ip ospf authentication-key jeremy
R2(config-if)#ip ospf authentication
R2(config-if)#
*Aug 23 04:56:28.435: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.1.1 on GigabitEthernet0/0 from FULL to DOWN, Neighbor Down: D
R2(config-if)#do show ip ospf neighbor
R2(config-if)#
```

7. IP MTU settings must match.
   1. Can become OSPF neighbors, but OSPF does not operate properly.

## LSA Types

<figure><img src=".gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

* **Type 1 (Router LSA)**
  * Every OSPF router generates this type of LSA.
  * It identifies the router using its router ID.
  * It also lists networks attached to the router's OSPF-activated interfaces.
* **Type 2 (Network LSA)**
  * Generated by the DR of each 'multi-access' network (ie. the broadcast network type).
  * Lists the routers which are attached to the multi-access network.
* **Type 5 (AS-External LSA)**
  * Generated by ASBRs to describe routes to destinations outside of the AS (OSPF domain).
