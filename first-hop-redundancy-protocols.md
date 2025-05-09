---
icon: frog
---

# First Hop Redundancy Protocols

<figure><img src=".gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
A first hop redundancy protocol (FHRP) is a computer networking protocol which is designed to protect the default gateway used on a subnetwork by allowing two or more routers to provide backup for that address; in the event of failure of an active router, the backup router will take over the address, usually within a few seconds.
{% endhint %}

> The two routers share a VIP, a virtual IP address, for example 172.16.0.252. You configure the PCs in the network to use that virtual IP as their default gateway, instead of the actual IP address of R1.

_<mark style="color:blue;">**Gratuitous ARP**</mark>_: ARP replies sent (broadcasted) without being requested (no ARP request message was received)

FHRPs are 'non-preemptive'. The current active router will not automatically give up its role, even if the former active router returns, but you can change this setting to make R1 preempt R2 and take back its active role automatically.

* A **virtual IP** is configured on the two routers, and a **virtual MAC** is generated for the virtual IP (each FHRP uses a different format for the virtual MAC).
* An **active** router and a **standby** router are elected. (Different FHRPs use different terms.)
* End hosts in the network are configured to use the virtual IP as their default gateway.
* The active router replies to ARP requests using the virtual MAC address, so traffic destined for other networks will be sent to it.
* If the active router fails, the standby becomes the next active router. The new active router will send **gratuitous ARP** messages so that switches will update their MAC address tables. It now functions as the default gateway.
* If the old active router comes back online, by default it won’t take back its role as the active router. It will become the standby router.
* You can configure _preemption_, so that the old active router does take back its old role.

***

## HSRP (Hot Standby Router Protocol)

* Cisco proprietary.
* An active and standby router are elected.
* There are two versions of HSRP, version 1 and version 2.
  * Version 2 adds IPv6 support, and increases the number of ‘groups’ that can be configured.
  * Multicast IPv4 address: 224.0.0.2 for version 1, and 224.0.0.102 for version 2.
  * Virtual Mac address: 0000.0c07.acXX (XX = HSRP group number) for version 1, and 0000.0c9f.fXXX (XXX = HSRP group number).
  * In a situation with multiple subnets/VLANs, you can configure a different active router in each subnet to load balance.
* The active router is determined in this order:
  * Highest priority (default 100)
  * Highest IP address
* Preempt causes the router to take the role of active router, even if another router already has the role.

### HSRP Configuration

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
R1(config-if)# standby version 2
R1(config-if)# standby 1 ip 172.16.0.254  
R1(config-if)#  
R1(config-if)# standby 1 priority ?  
  <0-255>  Priority value  
R1(config-if)# standby 1 priority 200  
R1(config-if)#  
R1(config-if)# standby 1 preempt  !only necessary on the router you want to become active
R1(config-if)#  
```

## VRRP (Virtual Router Redundancy Protocol)

* Unlike HSRP, it’s an open standard, so it can be run on devices from any maker. Cisco routers run it too, so you can use either HSRP or VRRP.
* Instead of an active and standby router, a **master** and **backup** router are elected.
* The IPv4 multicast address used is different as well, 224.0.0.18.
* The virtual MAC address format is different, too: 0000.5e00.01XX (XX = VRRP group number).
* In a situation with multiple subnets/VLANs, you can configure a different master router in each subnet to load balance.

## GLBP (Gateway Load Balancing Protocol)

* Cisco proprietary
* Here’s the big difference: it load balances among multiple routers within a single subnet.
* In GLBP, a single AVG, Active Virtual Gateway, is elected for the subnet.
* Then, up to four AVFs, active virtual forwarders, are assigned by the AVG, and the AVG itself can be an AVF also.
* Each AVF acts as the default gateway for a portion of the hosts in the subnet.
* So, load balancing is achieved within a single subnet.&#x20;
* The multicast IPv4 address is 224.0.0.102, same as HSRP version 2.
* The virtual MAC address format is 0007 b400, followed by the GLBP group number and the AVF number.
  * 0007.b4000.XXYY (XX = GLBP group number, YY = AVF number)

***

| FHRP | Terminology    | Multicast IP                  | Virtual MAC                           | Cisco proprietary? |
| ---- | -------------- | ----------------------------- | ------------------------------------- | ------------------ |
| HSRP | Active/Standby | v1: 224.0.0.2 v2: 224.0.0.102 | v1: 0000.0c07.acXX v2: 0000.0c9f.fXXX | Yes                |
| VRRP | Master/Backup  | 224.0.0.18                    | 0000.5e00.01XX                        | No                 |
| GLBP | AVG / AVF      | 224.0.0.102                   | 0007.b400.XXYY                        | Yes                |
