---
icon: arrow-progress
---

# Routing fundamentals

> What is routing?

When a router receives a packets, is its job to forward the packets to the correct destination.

* Routing is the process that routers use to determine the path that IP packets should take over a network to reach their destination.
  * Routers store routes to all of their known destinations in a <mark style="color:red;">routing table</mark>.
  * When routers receive packets, they look in the <mark style="color:red;">routing table</mark> to find the best route to forward that packet.
* There are two main routing methods:&#x20;
  * One is ‘dynamic routing’, in which routers use dynamic routing protocols such as OSPF to share routing information with each other automatically and build their routing tables.
  * Another type is, static routing. In this case, a network engineer or admin manually configures routes on the router.

A <mark style="color:blue;">route</mark> tells the router: to send a packet to destination X, you should send the packet to <mark style="color:blue;">next-hop</mark> Y.

{% hint style="info" %}
next-hop = the next router in the path to the destination
{% endhint %}

Or, if the destination is directly connected to the router, send the packet directly to the destination.

Or, if the destination is the router's own IP adress, receive the packet for yourself (do not forward it).

<figure><img src=".gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

### Routing table&#x20;

To view the routing table, one can use use the command:

```c
R1# show ip route

Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
           D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
           N - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
           E1 - OSPF external type 1, E2 - OSPF external type 2
           I1 - IS-IS, su - IS-IS summary, I1 - IS-IS level-1, L - IS-IS level-2
           IS-IS inter area, * - candidate default, U - per-user static route
           ODR - periodic downloaded static route, H - NHRP, I - LISP application route
           + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C     192.168.1.1/32 is directly connected, GigabitEthernet0/2
L     192.168.1.1/32 is directly connected, GigabitEthernet0/2
192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
C     192.168.12.1/32 is directly connected, GigabitEthernet0/1
L     192.168.12.1/32 is directly connected, GigabitEthernet0/1
192.168.13.0/24 is variably subnetted, 2 subnets, 2 masks
C     192.168.13.1/32 is directly connected, GigabitEthernet0/0
L     192.168.13.1/32 is directly connected, GigabitEthernet0/0
192.168.14.0/24 is directly connected, GigabitEthernet0/2
L     192.168.14.1/32 is directly connected, GigabitEthernet0/2
192.168.15.0/24 is directly connected, GigabitEthernet0/0
L     192.168.15.1/32 is directly connected, GigabitEthernet0/0
```

* The <mark style="color:purple;">Codes</mark> legend in the output of this command, lists the different protocols in which routers can use to learn routes.
* L - local
  * A route to the actual IP address configured on the interface (with a /32 netmask)
* C - connected
  * A route to the network the interface is connected to (with the actual netmask configured on the interface)
* When you configure an IP address on an interface and enable it no shutdown, 2 routes (per interface) will automatically be added to the routing table:
  * a connected route
  * a local coute

A connected route is a route to the network the interface is connected to.

* &#x20;R1 G0/2 IP = <mark style="color:red;">192.168.1</mark>.1/24
* Network Address = <mark style="color:red;">192.168.1.</mark>0/24
* It provides a route to all hosts in that network;
  * R1 knows: "If I need to send a packet to any host in the 192.168.1.0/24 network, I should send it out G0/2"

A local route is a route to the exact IP address configured on the interface.

* A /32 netmask is used to specify the exact IP address of the interface.
* Even though R1's G0/2 is configured as 192.168.1.1/24, the connected route is 192.168.1.1/32
  * R1 knows: "If I receive a packet destined for this IP adress, the message is for me".

A route matches a packet's destination if the packet's destination IP address is a part of the network specified in the route. If there are no matching routes, the packet will be dropped.

### Route selection

If there are more routes suitable for the packet's destination IP address, the router will choose the <mark style="color:red;">most specific</mark> matching route.

* Most specific matching route = the <mark style="color:yellow;">matching route</mark> with the **longest prefix length**.
