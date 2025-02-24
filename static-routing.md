---
icon: route
---

# Static Routing

<figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

If R2 receives packets with the destination address different than the neighborhed networks and routers, it will drop it, because it the routing table does not contain a route to the specific network.&#x20;

## Default Gateway

* End hosts like PC1 and PC4 can send packets directly to destinations in their connected network
  * To send packets to destinations outside of their local network, they must send the packets to their <mark style="color:red;">default gateway</mark>.
    * It is a route to 0.0.0.0/0 = all netmask bits set to 0. Includes all addresses from 0.0.0.0 to 255.255.255.255.
  * You can express the default gateway as default router.
* End hosts have no need for any more specific routes.
  * They just need to know: to send packets outside of my local network, I should send them to my default gateway.

## Static Routes

* When R1 receives the frame from PC1, it will de-encapsulate it (remove L2 header/trailer) and look at the inside packet.
* It will check the routing table for the most-specific matching route.
  * R1 has no matching routes in its routing table.
    * Which means, it will drop the packet.
* To properly forward the packet, R1 needs a route to the destination network (192.168.4.0/24)
  * Routes are instructions: TO send a packet to destinations in the network 192.168.4.0/24, forward the packet to next hop Y.
    * There are two possible paths packets from PC1 to PC4 can take.
      * 1\) PC1 - R1 - R3 - R4 - PC4
      * 2\) PC1 - R1 - R2 - R4 - PC4
    * It is possible to configure the routers to:
      * Load-balance between path 1) and 2)
      * Use path 1) as the main patch and path 2) as a backup path
* Each router in the path needs two routes: a route to 192.168.1.0/24 and a route to 192.168.4.0/24
  * This ensures two-way reachability (PC1 can send packets to PC4, PC4 can send packets to PC1)

R1 already has a Connected route to 192.168.1.0/24. R4 already has a Connected route to 192.168.4.0/24.

* The other routes must be manually configured (using Static routes)

<figure><img src=".gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

&#x20;

## Static Route Configuration

* The command for adding a static route is:

<pre><code>R1(config)# <a data-footnote-ref href="#user-content-fn-1">ip route</a> ip-address net-mask next-hop
</code></pre>

<figure><img src=".gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Static Route Configuration with exit-interface

Instead of configuring with a next-hop, you can configure it with an <mark style="color:red;">exit-interface</mark>.

For example, if R2 wants to send a packet to the 192.168.1.0/24 network, the exit-interface is G0/0, because that is the one connected to the G0/1 of the destination network.

You can set up both the next-hop and exit-interface.

```
R2(config)# ip route ip-address netmask exit-interface
R2(config)# ip route ip-address netmask exit-interface next-hop
```

<figure><img src=".gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

When you configure the static route with the _exit-interface_ option, it will be described as a directly connected route, but it is a static route.

* Static routes in which you specify only the exit-interface rely on a feature called <mark style="color:red;">Proxy ARP</mark> to function.
* This is usually not a problem, but generally you can stick to _next-hop_ or _exit-interface next-hop_

## Default Route

* A default route is a route to 0.0.0.0/0.
  * Is the least specific lest route possible; it includes every possible destination IP address.
  * If the router does not have any more specific routes that match a packet's destination IP address, the router will forward the packet using the default route.
  * Is often used to direct traffic to the Internet.

<figure><img src=".gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

* Gateway of last resort is not set
  * Which means that no default reoute has been configured yet.

In order to configure a default router, one has to type the command:

```
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>



[^1]: 
