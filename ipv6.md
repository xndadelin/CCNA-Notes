---
icon: '6'
---

# IPv6

## Why IPv6

* The main reason is that there simply aren’t enough IPv4 addresses are available.
* When IPv4 was being designed 30 years ago, the creators had no idea the Internet would be as large as it is today, they thought 32 bits would provide more than enough addresses.
* However, we have known about the IPv4 address exhaustion problem for a long time, and several techniques have been used to preserve the space.&#x20;
  * VLSM, variable-length subnet masks is one of the techniques that allows IPv4 address space to be preserved.&#x20;
  * Private IPv4 addresses and NAT, Network Address Translation, are two others that have made a huge difference as well.
* Those techniques have been very useful in preserving the IPv4 address space, however they are just short-term solutions.&#x20;
* The long-term solution is to transition to IPv6.
* IPv4 address assignments are controlled by IANA, the Internet Assigned Numbers Authority.
* IANA distributes IPv4 address space to various RIRs, Regional Internet Registries, which then assign them to companies that need them.&#x20;
  * them. For example, an Internet service provider would ask its local RIR to assign it IP addresses which can then be used by its customers.

## IPv6

* An IPv6 address is 128 bits. That’s 4 times the number of bits in an IPv4 address, which is 32 bits. At first, you might think that 4 times the number of bits means that there are 4 times the number of addresses. That’s wrong. Every additional bit DOUBLES the number of possible addresses.
* 32 bits allows for about 4 billion addresses. 33 bits would allow about 8 billion, 34 bits would allow about 16 billion, etc. So, how many IPv6 addresses are there? There are 340 undecillion, 282 decillion, 366 noncillion, 920 octillion, 938 septillion, 463 sextillion, 463 quintillion, 374 quadrillion, 607 trillion, 431 billion, 768 million, 211 thousand and 456 IPv6 addresses.

$$
340,282,366,920,938,463,463,374,607,431,768,211,456
$$

* IPv6 addresses aren’t written in dotted decimal, they are written in hexadecimal.
* An IPv6 address is 128 bits, each hexadecimal digit contains 4 bits of information. 128 bits divided by 4 is 32. So, an IPv6 address is written as 32 hexadecimal characters, divided into 8 groups of 4 using colons
* IPv6 addresses use the ‘slash’ notation to indicate the prefix length, even when configuring the address in the Cisco IOS CLI. No more dotted decimal subnet masks. This /64, for example, means the first half of the address would be the network portion, and the second half would be the host portion.

### Shortening (abbreviating) IPv6 addresses

* **Leading 0s** can be removed.
  * 2001:<mark style="color:blue;">**0**</mark>DB8:<mark style="color:blue;">000</mark>A:<mark style="color:blue;">00</mark>1B:20A1:<mark style="color:blue;">00</mark>20:<mark style="color:blue;">00</mark>80:34BD
  * The address can written like this:
    * 2001:DB8:A:1B:20A1:20:80:34BD
* Consecutive quartets of all 0s can be replaced with a double colon.
  * 2001: ODB8:9000:<mark style="color:blue;">0000:0000:0000</mark>:0080:34BD
  * 2001:0DB8::0080:34BD
  * Why are you able to do this? It’s because we know an IPv6 address is 8 quartets in length. We can only see four quartets now, so we know the double colon means that there are four quartets of all 0s.
* You can combine both methods.
  * But there are limitations:
    * Consecutive quartets of 0s can only be abbreviated once in an IPv6 address.

## Finding the IPv6 prefix (global unicast addresses)

* Typically, an enterprise requesting IPv6 addresses from their ISP will receive a /48 block.&#x20;
* Also, typically IPv6 subnets use a /64 prefix length.
* This means that an enterprise has 16 bits to use to make subnets.&#x20;
* And the remaining 64 bits can be used for hosts.

> <mark style="color:blue;">2001:ODB8:8B00</mark>:<mark style="color:purple;">0001</mark>:<mark style="color:orange;">0800:0000:0000:0001</mark>/64

* <mark style="color:red;">48-bit 'global routing prefix' assigned by the ISP</mark>
* <mark style="color:red;">16-bit 'subnet identifier', used by the enterprise to make various subnets</mark>
* <mark style="color:red;">64-bit 'interface identifier', the host portion of the address</mark>
* This part in blue is the /48 block assigned by the ISP, it’s called the ‘global routing prefix’.
* Note that this example is for the IPv6 ‘global unicast’ address type.
* The next 16 bits, 4 hex digits, are called the ‘subnet identifier’.&#x20;
* Because the enterprise received a /48 block from the ISP, but IPv6 addresses usually use a /64 prefix length, these 16 bits are free to use to make different subnets.
* Together, these two parts make the ‘network portion’ of the address, the IPv6 network prefix.
* Then the last 64 bits are the host bits. That is a huge amount of hosts per subnet.

## Finding the IPv6 prefix

<figure><img src=".gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

* Even if the prefix length isn’t /64, if the prefix length is a multiple of 4 it’s easy to find the prefix length. Why is that? It’s because each hexadecimal character is 4 bits. 56 is a multiple of 4, so let me show you how to find the prefix of this IPv6 address. This first quartet is the first 16 bits of the address. This one brings it to 32 bits. 48 bits. This 2 contains the next 4 bits, so 52. And this 1 contains another 4 bits, so 56 bits. So, these first 14 characters are the network portion of the address, the prefix. Everything after is the host portion, so we can change them all to 0 to find the prefix.

<figure><img src=".gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

## Configuring IPv6 addresses

<figure><img src=".gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

```
R1(config)#
R1(config)# ipv6 unicast-routing
R1(config)#
R1(config)# int g0/0
R1(config-if)# ipv6 address 2001:db8:0:0::1/64
R1(config-if)# no shutdown
R1(config-if)#
R1(config-if)# int g0/1
R1(config-if)# ipv6 address 2001:db8:0:1::1/64
R1(config-if)# no shutdown
R1(config-if)#
R1(config-if)# int g0/2
R1(config-if)# ipv6 address 2001:db8:0000:0002:0000:0000:0000:0001/64
R1(config-if)# no shutdown
R1(config-if)#
```

* This command allows the routers to perform IPv6 routing.

```
R1(config)# ipv6 unicast-routing
```

## Configuring IPv6 addresses (EUI-64)

* EUI stands for Extended Unique Identifier
* EUI-64 is a method of converting a 48-bit MAC address into a 64-bit interface identifier, and this interface identifier can then become the ‘host portion’ of a /64 IPv6 address.
* How to convert the MAC address:
  * Divide the MAC address in half
  * 1234 5678 90AB -> 1234 56 | 78 90AB
  * Insert FFFE in the middle
    * <mark style="color:blue;">1234 56</mark><mark style="color:yellow;">FF FE</mark><mark style="color:blue;">78 90AB</mark>
  * Invert the 7th bit.
    * <mark style="color:blue;">1</mark><mark style="color:red;">2</mark><mark style="color:blue;">34 56</mark><mark style="color:yellow;">FF FE</mark><mark style="color:blue;">78 90AB</mark>
      * <mark style="color:blue;">00</mark><mark style="color:orange;">1</mark><mark style="color:blue;">0 -> 0000 ⇒ 1</mark><mark style="color:red;">0</mark><mark style="color:blue;">34 56</mark><mark style="color:yellow;">FF FE</mark><mark style="color:blue;">78 90AB</mark>

```
R1(config)#int g0/0
R1(config-if)#ipv6 address 2001:db8::/64 eui-64
R1(config-if)#no shutdown

R1(config-if)#int g0/1
R1(config-if)#ipv6 address 2001:db8:0:1::/64 eui-64
R1(config-if)#no shutdown

R1(config-if)#int g0/2
R1(config-if)#ipv6 address 2001:db8:0:2::/64 eui-64
R1(config-if)#no shutdown
```

### Why invert the 7th bit?

* MAC addresses can be divided into two types:
  * **UAA** (Universally Administered Address)
    * Uniquely assigned to the device by the manufacturer
  * **LAA** (Locally Administered Address)
    * Manually assigned by an admin (with the `mac-address` command on the interface) or protocol. Doesn’t have to be globally unique.
* You can identify a UAA or LAA by the 7th bit of the MAC address, called the U/L bit (Universal/Local bit):
  * U/L bit set to `0` = **UAA**
  * U/L bit set to `1` = **LAA**
* In the context of IPv6 addresses/EUI-64, the meaning of the U/L bit is reversed:
  * U/L bit set to `0` = The MAC address the EUI-64 interface ID was made from was an **LAA**
  * U/L bit set to `1` = The MAC address the EUI-64 interface ID was made from was a **UAA**

## Global unicast addresses

* Global unicast IPv6 addresses are public addresses which can be used over the Internet.
* You must register to use global unicast addresses. Because they are public addresses, it is expected that they are globally unique. If two companies use the same global unicast address range, there are going to be problems, like two homes having the exact same street address.
* The range of addresses to be used for global unicast addresses was originally defined as\
  2000::/3, which includes all addresses from 2000, followed by 7 quartets of 0s, through 3FFF,\
  followed by seven quartets of Fs.
* Now all addresses which aren’t reserved for other purposes are global\
  unicast addresses.

## Unique local addresses

* Unique local IPv6 addresses are private addresses which cannot be used over the Internet.
* You do not need to register to use them. They can be used freely within internal networks and don’t need to be globally unique.
* Note that these addresses can’t be routed over the Internet, your ISP will simply drop packets destined for unique local addresses.
* The address block FC00::/7 is reserved for unique local addresses, however a later update requires\
  that the 8th bit be set to 1, so really all unique local addresses will begin with FD.
* The global ID should be unique so that addresses don’t overlap when companies merge.

FD45:93AC:8A8F:0001:0000:0000:0001/64

* **Indicates a unique local address**
* **40-bit 'global ID'**, which should be randomly generated
* **16-bit 'subnet identifier'**, used by the enterprise to make various subnets
* **64-bit 'interface identifier'**, the host portion of the address

## Link local addresses

* Link local IPv6 addresses are automatically generated on IPv6-enabled interfaces
* Use this command to enable IPv6 on an interface.

```
R1(config-if)#ipv6 enable
```

* Link local addresses use the address block FE80::/10.
* However, the standard states that the 54 bits after FE80/10 should all be 0, so you won’t actually see any link local addresses beginning with FE9, FEA, or FEB. Only FE8.
* Then, the interface ID is generated using EUI-64 rules.
* Link local means that these addresses are used for communication within a single link, a single subnet. Routers will not route packets with a link-local destination IPv6 address, they will not forward the packets between subnets.
* So, what are some actual uses of link-local addresses?
  * For example, routing protocol peerings. OSPFv3, used for IPv6, uses link-local addresses for neighbor adjacencies, sending LSAs, etc.
  * They can also be used as the next-hop address for static routes.
  * And neighbor discovery protocol, NDP, which is IPv6’s replacement for ARP, uses link-local addresses to function.

## Multicast addreses

* **Unicast** addresses are one-to-one.
  * One source to one destination.
* **Broadcast** addresses are one-to-all.
  * One source to all destinations (within the subnet).
* **Multicast** addresses are one-to-many.
  * One source to multiple destinations (that have joined the specific multicast group).
* IPv6 uses range `FF00::/8` for multicast. (`FF00::` to `FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF`)
* IPv6 doesn’t use broadcast (there is no 'broadcast address' in IPv6!)

| Purpose                                    | IPv6 Address | IPv4 Address |
| ------------------------------------------ | ------------ | ------------ |
| All nodes/hosts (functions like broadcast) | FF02::1      | 224.0.0.1    |
| All routers                                | FF02::2      | 224.0.0.2    |
| All OSPF routers                           | FF02::5      | 224.0.0.5    |
| All OSPF DRs/BDRs                          | FF02::6      | 224.0.0.6    |
| All RIP routers                            | FF02::9      | 224.0.0.9    |
| All EIGRP routers                          | FF02::A      | 224.0.0.10   |

### Multicast addresses scopes

IPv6 defines multiple multicast ‘scopes’ which indicate how far the packet should be forwarded. The addresses in the previous slide all use the ‘link-local’ scope (FF02), which stays in the local subnet.

#### IPv6 multicast scopes:

* **Interface-local (FF01)**: The packet doesn’t leave the local device. Can be used to send traffic to a service within the local device.
* **Link-local (FF02)**: The packet remains in the local subnet. Routers will not route the packet between subnets.
* **Site-local (FF05)**: The packet can be forwarded by routers. Should be limited to a single physical location (not forwarded over a WAN).
* **Organization-local (FF08)**: Wider in scope than site-local (an entire company/organization).
* **Global (FF0E)**: No boundaries. Possible to be routed over the Internet.

<figure><img src=".gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>

## Anycast addresses

* **Anycast** is a new feature of IPv6.
* **Anycast** is ‘one-to-one-of-many’.
* Multiple routers are configured with the same IPv6 address:
  * They use a routing protocol to advertise the address.
  * When hosts send packets to that destination address, routers will forward it to the nearest router configured with that IP address (based on routing metric).
* There is no specific address range for anycast addresses. Use a regular unicast address (global unicast, unique local) and specify it as an anycast address:

```bash
R1(config-if)# ipv6 address 2001:db8:1:1::99/128 anycast
```

## Other IPv6 addresses

* `::` = The unspecified IPv6 address\
  ↳ Can be used when a device doesn’t yet know its IPv6 address.\
  ↳ IPv6 default routes are configured to `::/0`\
  ↳ IPv4 equivalent: `0.0.0.0`
* `::1` = The loopback address\
  ↳ Used to test the protocol stack on the local device.\
  ↳ Messages sent to this address are processed within the local device, but not sent to other devices.\
  ↳ IPv4 equivalent: `127.0.0.0/8` address range

## RFC 5952

{% hint style="info" %}
RFC 5952 suggests standardizing IPv6 address representation.
{% endhint %}

* Leading 0s MUST be removed. 2001:0db8:0000:0001:0f2a:4fff:fea3:00b1 → 2001:db8:0:1:f2a:4fff:fea3:b1
* `::` MUST be used to shorten the longest string of all-0 quartets.\
  (If there is only one all-0 quartet, don’t use `::`) 2001:0000:0000:0000:0f2a:0000:0000:00b1 → 2001::f2a:0:0:b1
* If there are two equal-length choices for `::`, use `::` to shorten the one on the left. 2001:0db8:0000:0000:0f2a:0000:0000:00b1 → 2001:db8::f2a:0:0:b1
* Hexadecimal characters 'a', 'b', 'c', 'd', 'e', and 'f' MUST be written using lower-case, NOT upper-case A B C D E F.

## IPv6 Header

* Fixed size of 40 bytes.

<figure><img src=".gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

* **Version**
  * Length: 4 bits
  * Indicates the version of IP that is used.
  * Fixed value of 6 (0b0110) to indicate IPv6.
* **Traffic Class**
  * Length: 8 bits
  * Used for QoS (Quality of Service), to indicate high-priority traffic.
  * For example, IP phone traffic, live video calls, etc., will have a Traffic Class value which gives them priority over other traffic.
* **Payload Length**
  * Length: 16 bits
  * Indicates the length of the payload (the encapsulated Layer 4 segment) in bytes.
  * The length of the IPv6 header itself isn’t included, because it’s always 40 bytes.
* **Next Header**
  * Length: 8 bits
  * Indicates the type of the ‘next header’ (header of the encapsulated segment), for example, TCP or UDP.
  * Same function as the IPv4 header’s 'Protocol' field.
* **Hop Limit**
  * Length: 8 bits
  * The value in this field is decremented by 1 by each router that forwards it. If it reaches 0, the packet is discarded.
  * Same function as the IPv4 header’s 'TTL' field.
* **Source Address**
  * Length: 128 bits
  * These fields contain the IPv6 addresses of the packet’s source.
* **Destination Address**
  * Length: 128 bits
  * These fields contain the IPv6 addresses of the packet’s intended destination.

## Solicited-Node Multicast Address

An IPv6 solicited-node multicast address is calculated from a unicast address.

ff02:0000:0000:0000:0001:ff + Last 6 hex digits of unicast address

1. Unicast address: `2001:0db8:0000:0001:0f2a:4fff:fea3:00b1`
   * Solicited-Node Multicast address: `ff02::1:ff:a3:b1`
2. Unicast address: `2001:0db8:0000:0001:0489:4eda:073a:12b8`
   * Solicited-Node Multicast address: `ff02::1:ff:3a:12b8`

## Neighbor Discovery Protocol

* Neighbor Discovery Protocol (NDP) is a protocol used with IPv6.
* It has various functions, and one of those functions is to replace ARP, which is no longer used in IPv6.
* The ARP-like function of NDP uses ICMPv6 and solicited-node multicast addresses to learn the MAC address of other hosts.\
  &#xNAN;_(ARP in IPv4 uses broadcast messages)_.
* Two message types are used:
  1. Neighbor Solicitation (NS) = ICMPv6 Type 135
  2. Neighbor Advertisement (NA) = ICMPv6 Type 136

<figure><img src=".gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

### IPv6 Neighbor Table

<figure><img src=".gitbook/assets/image (156).png" alt=""><figcaption></figcaption></figure>

### NDP Router Discovery Function

* Another function of NDP allows hosts to automatically discover routers on the local network.
* Two messages are used for this process:
  1. **Router Solicitation (RS) = ICMPv6 Type 133**
     * Sent to multicast address FF02::2 (all routers).
     * Asks all routers on the local link to identify themselves.
     * Sent when an interface is enabled/host is connected to the network.
  2. **Router Advertisement (RA) = ICMPv6 Type 134**
     * Sent to multicast address FF02::1 (all nodes).
     * The router announces its presence, as well as other information about the link.
     * These messages are sent in response to RS messages.
     * They are also sent periodically, even if the router hasn’t received an RS.

### Stateless Address Auto-configuration (SLAAC)

* Stands for **Stateless Address Auto-configuration**.
* Hosts use the RS/RA messages to learn the IPv6 prefix of the local link (i.e. `2001:db8::/64`), and then automatically generate an IPv6 address.
* Using the `ipv6 address prefix/prefix-length eui-64` command, you need to manually enter the prefix.
* Using the `ipv6 address autoconfig` command, you don’t need to enter the prefix. The device uses NDP to learn the prefix used on the local link.
* The device will use EUI-64 to generate the interface ID, or it will be randomly generated (depending on the device/maker).
* Standard function of IPv6, not Cisco special.

## Duplicate Address Detection (DAD)

* **Duplicate Address Detection (DAD)** allows hosts to check if other devices on the local link are using the same IPv6 address.
* Any time an IPv6-enabled interface initializes (no `shutdown` command), or an IPv6 address is configured on an interface (by any method: manual, SLAAC, etc.), it performs DAD.
* DAD uses two messages you learned earlier: NS and NA.
* The host will send an NS to its own IPv6 address. If it doesn’t get a reply, it knows the address is unique.
* If it gets a reply, it means another host on the network is already using the address.

## IPv6 Static Routing

* **IPv6 routing** works the same as **IPv4 routing**.
* However, the two processes are separate on the router, and the two routing tables are separate as well.
* **IPv4 routing** is enabled by default.
* **IPv6 routing** is disabled by default, and must be enabled with `ipv6 unicast-routing`.
* If **IPv6 routing** is disabled, the router will be able to send and receive IPv6 traffic, but will not **route** IPv6 traffic (= will not forward it between networks
* A connected **network route** is automatically added for each connected network.
* A local **host route** is automatically added for each address configured on the router.
* Routes for link-local addresses are not added to the routing table.

```
ipv6 route destination/prefix-length {next-hop | exit-interface [next-hop]} [ad]
```

## Static Routes

#### Directly attached static route: Only the exit interface is specified.

```
ipv6 route destination/prefix-length exit-interface
```

* In IPv6, you cannot use directly attached static routes if the interface is an Ethernet interface.

#### Recursive static route: Only the next hop is specified.

```
ipv6 route destination/prefix-length next-hop
```

#### Fully specified static route: Both the exit interface and next hop are specified.

```
ipv6 route destination/prefix-length exit-interface next-hop
```

