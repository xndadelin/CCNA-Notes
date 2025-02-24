---
icon: arrow-down-up-across-line
---

# Dynamic Routing

* A network route is a route to a network/subnet with the mask length less than 32.
* A host route is a route to a specific host, specified with a /32 mask.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Instead of configuring static routes on each routers, you can enable the dynamic routing protocol. Then R4 wil advertise to its neighbor that it can reach 192.168.4.0/24 via itself. R2 will add this route to its routing table. R2 will advertise to its neighbor R1 that it can reach 192.168.4.0/24 via itself and so on.

* Routers can use dynamic routing protocols to advertise information about the routes they know to other routes.
* They form 'adjacensies' / 'neighbor relationships' / 'neighborships' with adjacent routers to exchange information.
* If multiple routes to a destination are learned, the router determines which route is superior and adds it to the routing table. It uses the 'metric' of the route to decide which is superior (lower metric = superior).

## Types of dynamic routing protocols

* Dynamic routing protocols can be divided into two main categories:
  * IGP (Interior Gateway Protocol)
    * Used to share routes within a single autonomous system (AS), which is a single organization (ie. a company).&#x20;
    * Uses 2 algoritms:
      * Distance Vector
        * Routing Information Protocol (RIP)
        * Enhance Interior Gateway Routing Protocol (EIGRP)
      * Link State
        * OSPF (Open Shortest Path First)
        * &#x20;IS-IS (Intermediate System to Intermediate System)
  * EGP (Exterior Gateway Protocol) &#x20;
    * Used to share routes between different autonomous systems.
    * Path Vector is an EGP algoritm. (Border Gateway Protocol BGP)

## Distance vector routing protocols

* Distance vector protocols were invented before link state protocols.
* Early examples are RIPv1 and Cisco's proprietary protocol IGRP (which was updated to EIGRP).
* Distance vector protocols operate by sending the following to their directly connected neighbors
  * &#x20;their known destination networks
  * their metric to reach their known destination networks
* &#x20;This method of sharing route information is often called 'routing by rumor'.&#x20;
  * This is because the router does not know about the network beyond its neighbors. It only knows the information that its neighbors tell it.
* Called 'distance vector' because the routers only learn the distance (metric) and 'vector' (direction, the next-hop counter) of each route.

## Link state routing protocols

* When using a **link state** routing protocol, every router creates a 'connectivity map' of the network.&#x20;
* To allow this, each router advertises information about its interfaces (connected networks) to its neighbors. These advertisements are passed along to other routers, until all routers in the network develop the same map of the network.
* Each router independently uses this map to calculate the best routes to each destination.
* Link state protocols use more resources (CPU) on the router, because more information is shared.
* However, link state protocols tend to be faster in reacting to changes in the network than distance vector protocols.

## Dynamic routing protocol metrics

* A router's route table contains the best route to each destination network it knows about.&#x20;
* If a router using a dynamic routing protocol learns two different routes to the same destination, how does it determine which is 'best' ?
* It uses the metric value of the routes to determine which is best. A lower metric means a better route.
* Each routing protocol uses a different metric to determine which route is the best.
* If a router learns two or more routes via the same routing protocol to the same destination with the same metric, both will be added to the routing table. Traffic will be load-balanced over both routes.&#x20;
* ECMP (Equal Cost Multi-Path) = METRIC.
* The OSPF protocol has an AD (administrative distance) of 110

| IGP   | Metric                              | Explanation                                                                                                                                                                |
| ----- | ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RIP   | Hop Count                           | Each router in the path counts as one 'hop'. The total metric is the total numbers of hops to the destination. Links of all speed are equal.                               |
| EIGRP | Metric based on bandwidth and delay | Complex formula that can take into account many values. By default, the bandwidth of the slowest link in the route and the total delay of all links in the route are used. |
| OSPF  | Cost                                | The cost of each link is calculated based on bandwidth. The total metric is the total cost of each link in the route.                                                      |
| IS-IS | Cost                                | The total metric is the total cost of each link in the route. The cost of each link is not automatically calculated by default. All links have a cost of 10 by default.    |

## Administrative distance

* In most cases, a company will use a single IGP - usually OSPF or EIGRP.&#x20;
* However, in some rare cases they might use two. For example, if two companies connect their network to share information, two different routing protocols might be in use.
* Metric is used to compare routes _**learned via the same routing protocol.**_
* &#x20;Different routing protocols use totally different metrics, so they cannot be compared.
* The AD is used to determine which routing protocol is preferred.
* A lower AD is preferred, and indicates that the routing protocol is considered more 'trustworthy' (more likely to select good routes).

| Route protocol/type  | AD  |
| -------------------- | --- |
| Directly connected   | 0   |
| Static               | 1   |
| External BGP (eBGP)  | 20  |
| EIGRP                | 90  |
| IGRP                 | 100 |
| OSPF                 | 110 |
| IS-IS                | 115 |
| RIP                  | 120 |
| EIGRP (external)     | 170 |
| Internal BGP (iBGP)  | 200 |
| Unusable route       | 255 |

* You can change the AD of a routing protocol.&#x20;
* By changing the AD of a static route, you can make it less preferred than routes learned by a dynamic routing protocol to the same destination. &#x20;
* This is called a 'floating static route'.
* The route will be inactive (not in the routing table) unless the route learned by the dynamic routing protocol is removed (for example, the remote router stops advertising it for some reason, or an interface failure causes an adjacency with a neighbor to be lost).
