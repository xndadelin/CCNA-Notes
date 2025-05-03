---
description: Stands for Virtual Local Area Network
icon: earth-asia
---

# VLANs



* A LAN is a single broadcast domain, including all devices in that broadcast domain.
* A broadcast domain is the group of devices which will receive a broadcast frame (destination MAC FFFF.FFFF.FFFF) sent by any one of the members)

<figure><img src=".gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

This is not necessarily the best setup, for both security and performance purposes. It would be best to separate this into separate subnets.

For example, let's say that one's PC from the Engineering department will send a broadcast frame. The switch will eventually flood all the interfaces except the one it was received on, meaning the frame will be received also by all PCs in the Sales and HR departments. This creates redundancy and form congestion.

When it comes to perfomance, lots of unnecessary broadcast traffic can reduce network perfomance.

As for security, even within the same office, you want to limit who has access to what. You can apply security policies on a router/firewall. Because this is one LAN, PCs can reach each other directly, without traffic passing through the router. So even if you configure security policies, they won't have any effect.&#x20;

## Segmenting at Layer 3 (Subnets)

<figure><img src=".gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

For each subnet, the router is required to have 3 separate interfaces.

When a PC from the engineering department tries to ping the sales department, the PC will recognize its from a different subnet, so it will send the frame to the default gateway. And then the router will forward the frame to the sales department. However, there is still a problem, if a PC from the engineering department sends a broadcast frame, the switch will forward it out of all interfaces, except it was received on. It cannot make the difference between subnets, because the switch only works at the Layer 2. The goal is that so only the engineering department will receive the broadcast frame.

* Although we separated the three departments into three subnets (Layer 3), they are still in the same broadcast domain (Layer 2)
  * One possible solution is to buy different switchs for each department. However, that is not very flexible and not cheap. This is where VLANs are coming. We use them to separate departments at layer 2.

<figure><img src=".gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

Assigning the hosts the the VLANs will be done by configuring them on switch, more specifically on the switch interfaces. We configure the switch interface to be in a specific VLAN and then the endhost connected to that interface is part of that VLAN. The switch will consider each VLAN as a separate LAN, and will not forward traffic between VLANs, including broadcast/unknown unicast traffic.

The switch does not perfom <mark style="color:red;">**inter-VLAN routing**</mark>. It must send the traffic through the router.

## VLANs...

* are configured on switched on a per-interface basis.
* logically separate end hosts at Layer 2.

Switches do not forward traffic directly between hosts in different VLANs. The switch must forward the frame to the router.&#x20;

## VLAN configuration

<figure><img src=".gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

### Default VLANs

```
SW1#show vlan brief
```

<figure><img src=".gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

* This command displays the VLANs configured on the switch and which interfaces are in each VLAN.
* VLAN1 - is the default VLAN and it includes all interfaces connected to the switch.

{% hint style="info" %}
VLANs 1, 1002 - 1005 exist by default and cannot be deleted.
{% endhint %}

### Assign interfaces to VLAN

<figure><img src=".gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

* First, you use the interface range command to configure the interfaces from g1/0 - 3.
* Use this command to set the interface as an access port
  * An access port is a switchport which belongs to a single VLAN, and usually connects to end hosts like PCs. It gives the end hosts access to the network.
  * Switchports which carry multiple VLANs are called 'trunk ports'.&#x20;

```
SW1(config-if-range)#switchport mode access
```

* This command assigns the VLAN to the switchport

```
SW1(config-if-range)#switchport access vlan 10
```

<figure><img src=".gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

To enter a VLAN's configuration mode:

```
SW1(config)#vlan
```

* This is the command that creates a VLAN. (In this case, it was assigned automatically created when we assigned the interface)

To name the VLAN:

```
SW1(config-vlan)#name ENGINEERING
```

<figure><img src=".gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

## Review of topology

<figure><img src=".gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
There is no link in VLAN20 between SW1 and SW2. This is because there are no PCs in VLAN20 connected to SW1. PCs in VLAN20 can still reach PCs connected to SW1, R1 will perform inter-VLAN routing.
{% endhint %}

## Trunk ports

* In a small network with few VLANs, it is possible to use a separate interface for each VLAN when connecting switches to switches, and switches to routers.
* However, when the number of VLANs increases, this is not viable. It will result in wasted interfaces, and often routers will not have enough interfaces for each VLAN.
* You can use <mark style="color:yellow;">trunk ports</mark> to carry traffic from multiple VLANs over a single interface.

<figure><img src=".gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

For example, if a PC from VLAN10(right) sends data to VLAN10(left), it will go trough SW1 and SW2. SW1 will know from which VLAN did the data come, but how does SW1 know it? The answer is:

### VLAN Tagging

* Switches will 'tag' all frames that they send over a trunk link. This allows the receiving switch to know which VLAN the frame belongs to.

⇒ Trunk ports = 'tagged ports'

⇒ Access ports = 'untagged ports'

* There are two main trunking protocols:
  * ISL (Inter-Switch Link)
    * Old Cisco proprietary protocol created before the industry standard IEEE 802.1Q
  * IEEE 802.1Q (usually called dot1q)
    * Industry standard protocol created by the IEEE (Institute of Electrical and Electronics Engineers)
    * The dot1q tag is inserted between two ethernet frame fields.
    * Is inserted between Source and Type/Length fields of the Ethernet frame.
    * The tag is 4 bytes(32 bits) in length.
    * The tag consists of two main fields:
      * Tag Protocol Identifier (TPID)
        * 16 bits (2 bytes) in length
        * Always set to 0x8100. This indicates that the frame is 802.1Q-tagged.
      * Tag Control Information (TCI)
        * Consists of three sub-fields&#x20;
          * PCP - Priority Code Point
            * 3 bits in length
              * Used for Class of Service (CoS), which prioritizes important traffic in congested network.
          * DEI - Drop Eligible Indicator
            * 1 bit in length
              * Used to indicate frames that can be dropped if the network is congested
          * VID - VLAN ID
            * 12 bits in length
              * Identifies the VLAN the frame belongs to
              * 12 bits in length = 4096 total VLANs (2^12), ranged from 0 - 4095
              * The first and last VLAN are reserved and cannot be used (0 & 4095)
              * Therefore, the actual range of VLAN is 1 - 4094
              * Cisco's proprietary ISL also has a VLAN range of 1 - 4094

<figure><img src=".gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

### VLAN Ranges

* The range of VLANs (1 - 4094) is divided into two sections:
  * Normal VLANs: 1 - 1005
  * Extended VLANs: 1006 - 4094
* Some older devices cannot use the extended VLAN range, however, it is safe to expect that modern switches will support the extended VLAN range.

### Native VLAN

* 802.1q has a feature called the native VLAN. (Cisco ISL does not have this feature)
* The native VLAN is VLAN1 by default on all trunk ports, however, this can be manually configured on each trunk port.
* The switch does not add an 802.1q tag to the frames in the native VLAN.
* When a switch receives an untagged frame on a trunk port, it assumes the frame belongs to the native VLAN. (It's very important that the native VLAN matches!)

### Trunk Configuration

<figure><img src=".gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

* Many modern switches do not support Cisco's ISL at all. They only support 802.1Q.
* However, switches that do support both (like the one used in this example) have a trunk encapsulation of 'Auto' by default
* To manually configure the interface as a trunk port, you must first set the encapsulation to 802.1Q or ISL. On switches that only support 802.1Q, this is not necessary.
* After you set the encapsulation type, you can then configure the interface as a trunk.

<figure><img src=".gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

| Option   | Syntax                                             | Explanation                                                                                                                        |
| -------- | -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| VLAN IDs | `switchport trunk allowed vlan 1,2,3,10-20`        | Specific VLAN IDs or ranges that are allowed on the trunk port. Multiple VLANs can be listed with commas, and ranges with hyphens. |
| add      | `switchport trunk allowed vlan add 4,5,6`          | Adds specified VLANs to the current list of allowed VLANs without removing existing ones.                                          |
| all      | `switchport trunk allowed vlan all`                | Allows all VLANs (1-4094) on the trunk port. This is the default setting on most switches.                                         |
| except   | `switchport trunk allowed vlan except 100,200-300` | Allows all VLANs except the ones specified. Useful when you want to block just a few VLANs.                                        |
| none     | `switchport trunk allowed vlan none`               | Blocks all VLANs on the trunk port. No traffic will pass.                                                                          |
| remove   | `switchport trunk allowed vlan remove 5,10-20`     | Removes specified VLANs from the current list of allowed VLANs without affecting other allowed VLANs.                              |

For security purposes, it is best to change the native VLAN to an unused VLAN. **Make sure the native VLAN matches on betweek switches.**

### **Change Native VLAN**

```
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# exit
```

{% hint style="info" %}
The show vlan brief command shows the access ports assigned to each VLAN, NOT the trunk ports that allow each VLAN.

Use the show interfaces trunk command instead to confirm trunk ports.
{% endhint %}

## Router-on-a-stick ROAS

There is one single interface connecting the router to the switch. The single interface is divided into three subinterfaces.

<figure><img src=".gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

* There are 3 logical subinterfaces, which makes up for one physical interface and they operate like 3 separate interfaces.

```typescript
R1(config)#interface g0/0
R1(config-if)#no shutdown
R1(config-if)#
*Apr 15 04:29:49.681: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
*Apr 15 04:29:50.682: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
R1(config-if)#interface g0/0.10
R1(config-subif)#encapsulation dot1q 10
R1(config-subif)#ip address 192.168.1.62 255.255.255.192
R1(config-subif)#interface g0/0.20
R1(config-subif)#encapsulation dot1q 20
R1(config-subif)#ip address 192.168.1.126 255.255.255.192
R1(config-subif)#interface g0/0.30
R1(config-subif)#encapsulation dot1q 30
R1(config-subif)#ip address 192.168.1.190 255.255.255.192
R1(config-subif)#
```

* The subinterface number does not have to match the VLAN number. However it is highly recommended that they do match, to make it easier to understand.
* ROAS is used to route between VLANS using a single interface on the router and switch.
* The router interface is configured using subinterfaces. You configure the VLAN tag and IP address on each subinterface.
* The router will behave as if frames arriving with a certain VLAN tag have arrived on the subinterface configured with that VLAN tag.
* The router will tag frames sent out out of each subinterface with the VLAN tag configured on the subinterface.
