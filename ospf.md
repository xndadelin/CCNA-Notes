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
