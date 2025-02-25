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
