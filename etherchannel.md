---
icon: ethernet
---

# EtherChannel

## Intro

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* ASW1 (Access layer switch) is a switch that end hosts connect to.
* DSW1 (Distribution layer switch) is a switch that access layer switches connect to.&#x20;
* If there are 40 hosts connected to ASW1, the connection is congested. The admin should add another link to increase the bandwidth, so it can support all of the end hosts.

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

The network is still congested, and users are reporting problems. SO, the admin adds another link.

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

When the bandwidth of the interfaces connected to end hosts is greater than the bandwidth of the connection to the distribution switch(es), this is called **oversubscription**.\
Some oversubscription is acceptable, but too much will cause congestion.

Even with three links, the congestion does not seem any better. SO it adds again another link.

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

If you connect two switches together with multiple links, all except one will be disabled by STP.

* If all of ASW1's interfaces were forwarding, Layer 2 loops would form between ASW1 and DSW1, leading to broadcast storms.
* Other links will be unused unless the active link fails. IN that case, one of the inactive links will start forwarding.&#x20;
* It's a waste of bandwidth to have these three interfaces disabled, not forwarding any traffic.
* However, by forming these four physical interfaces into one logical interface, EtherChannel can solve this problem, giving us redundancy and increased bandwidth.

An EtherChannel is represented in network diagrams by drawing a circle around the interfaces.

* EtherChannel groups multiple interfaces together to act as a single interface.
* STP will treat this group as a single interface.

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

* Traffic using the EtherChannel will be load balanced among the physical interfaces in the group. An algoritm is used to determine which traffic will use which physical interface.
* Other names for EtherChannel are:
  * Port Channel
  * LAG (Link Aggregation Group)

## Load Balancing

* EtherChannel load balances bases on 'flows'.
* A flow is a communication between two nodes in the network.
* Frames in the same flow will be forwarded using the same physical interface.
* If frames in the same flow were forwarded using different physical interfaces, some frames may arrive at the destination out of order, which can cause problems.

```
ASW1#show etherchannel load-balance  
EtherChannel Load-Balancing Configuration:  
src-dst-ip  

EtherChannel Load-Balancing Addresses Used Per-Protocol:
Non-IP: Source XOR Destination MAC address  
IPv4: Source XOR Destination IP address  
IPv6: Source XOR Destination IP address  
```

```
ASW1#conf t  
Enter configuration commands, one per line.  End with CNTL/Z.  

**ASW1(config)#port-channel load-balance src-dst-mac**  

ASW1(config)#do show etherchannel load-balance  
EtherChannel Load-Balancing Configuration:  
src-dst-mac  

**EtherChannel Load-Balancing Addresses Used Per-Protocol:**  
Non-IP: Source XOR Destination MAC address  
IPv4: Source XOR Destination MAC address  
IPv6: Source XOR Destination MAC address  

ASW1(config)#
```

```
ASW1(config)#port-channel load-balance ?
  dst-ip      Dst IP Addr
  dst-mac     Dst Mac Addr
  src-dst-ip  Src XOR Dst IP Addr
  src-dst-mac Src XOR Dst Mac Addr
  src-ip      Src IP Addr
  src-mac     Src Mac Addr
ASW1(config)#port-channel load-balan
```

## EtherChannel Configuration

### PAgP (Port Aggregation Protocol)

* PAgP is a Cisco proprietary protocol.
* It dynamically negotiates the creation/maintenance of EtherChannel.

```
ASW1(config)#interface range g0/0 - 3
ASW1(config-if-range)#channel-group 1 mode ?
  active    Enable LACP unconditionally
  auto      Enable PAgP only if a PAgP device is detected
  desirable Enable PAgP unconditionally 
  on        Enable Etherchannel only
  passive   Enable LACP only if a LACP device is detected

ASW1(config-if-range)#channel-group 1 mode desirable
Creating a port-channel interface Port-channel 1
////////////////////////////////////////////////////////////////////////////////
auto + auto = no EtherChannel
desirable + auto = EtherChannel
desirable + desirable = EtherChannel
```

```
SW(config-if)# channel-group <number> mode <mode>
```

The channel-group number has to match the member interfaces on the same switch. However, it does not have to match the channel-group number on the other switch. (channel-group 1 on ASW1 can form an EtherChannel with channel-group 2 on DSW1)

### LACP (Link Aggregation Control Protocol)

* Industry standard protocol (IEEE 802.3ad)
* It dynamically negotiates the creation/maintenance of EtherChannel.

```
ASM1(config-if-range)#channel-group 1 mode ?
active     Enable LACP unconditionally
auto       Enable PAgP only if a PAgP device is detected
desirable  Enable PAgP unconditionally 
on         Enable Etherchannel only
passive    Enable LACP only if a LACP device is detected

ASM1(config-if-range)#channel-group 1 mode active
Creating a port-channel interface Port-channel 1
////////////////////////////////////////////////////////////////////////////////
passive + passive = no EtherChannel
passive + active = EtherChannel
active + active = EtherChannel
```

### Static EtherChannel

* A protocol is not used to determine if an EtherChannel should be formed.
* Interfaces are statically configured to form an EtherChannel.

Up to 8 interfaces can be formed into a single EtherChannel (LACP allows up to 16, but only 8 will be active, the other 8 will be in standby mode, waiting for an active interface to fail).

* &#x20;There aren't two separate modes, just one, 'ON' which manually tells these interfaces to form an EtherChannel.
* On mode only works with on mode (on + desirable or on + active will not work).

### Configure port-channel interface

```
ASM1(config)#interface port-channel 1
ASM1(config-if)#switchport trunk encapsulation dot1q
ASM1(config-if)#switchport mode trunk
ASM1(config-if)#do show interfaces trunk

Port        Mode         Encapsulation  Status        Native vlan
Po1         on          802.1q         trunking      1

Port        Vlans allowed on trunk
Po1         1-4094

Port        Vlans allowed and active in management domain
Po1         1

Port        Vlans in spanning tree forwarding state and not pruned
Po1         1
```

* Member interfaces must have matching configurations.
  * Same duplex (full/half)
  * Same speed
  * Same switchport mode (access/trunk)
  * Same allowed VLANs/native VLAN (for trunk interfaces)&#x20;
* If an interface's configurations does not match the others, it will be excluded from the EtherChannel.

```
ASW1# show etherchannel summary
ASW1# show etherchannel port-channel
```

### Manually configure the negotiation protocol

* Not a very useful command, because there is no need to configure it.

```
ASM1(config-if-range)#channel-protocol ?
lacp  Prepare interface for LACP protocol
pagp  Prepare interface for PAgP protocol

ASM1(config-if-range)#channel-protocol lacp
ASM1(config-if-range)#channel-group 1 mode desirable
Command rejected (Channel protocol mismatch for interface Gi0/0 in group 1): the interface can not be added to the channel group
% Range command terminated because it failed on GigabitEthernet0/0
ASM1(config-if-range)#channel-group 1 mode on
Command rejected (Channel protocol mismatch for interface Gi0/0 in group 1): the interface can not be added to the channel group
% Range command terminated because it failed on GigabitEthernet0/0
ASM1(config-if-range)#channel-group 1 mode active
Creating a port-channel interface Port-channel 1
ASM1(config-if-range)#
```

## Layer 3 EtherChannels

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

```
ASM1(config)#int range g0/0 - 3
ASM1(config-if-range)#no switchport
ASM1(config-if-range)#channel-group 1 mode active
Creating a port-channel interface Port-channel 1
```

```
ASM1(config-if-range)#int po1
ASM1(config-if)#ip address 10.0.0.1 255.255.255.252
ASM1(config-if)#
```

## Commands

```
SW(config) port-channel load-balance mode
# configures the EtherChannel load-balancing method on the switch

SW# show etherchannel load-balance
# displays information about the load-balancing settings

SW(config-if)# channel-group number mode {desirable|auto|active|passive|on}
# configures an interface to be part of an EtherChannel

SW# show etherchannel summary
# displays a summary of EtherChannels on the switch

SW# show etherchannel port-channel
# displays information about the virtual port-channel interfaces on the switch
```
