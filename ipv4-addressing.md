---
icon: '4'
description: This is based on the Layer 3 of the OSI model, where routers operate.
---

# IPv4 Addressing

## Routing

The broadcast of frames to end devices is limited to the local network, it does not cross the router and cannot go to another end device.

## IPv4

<figure><img src=".gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

* Source IP Address & Destination IP Address
  * These fields are both 32-bits (4 bytes) in length, as you can see they stretch from 0 to 31 in this chart.

{% hint style="info" %}
192.169.1.254 - an IPv4 Address
{% endhint %}

* An IPv4 address is 32 bits long, so each of these four groups of numbers represents 8 bits. 192 represents 8 bits, 168 - 8 bits, 1 - 8 bits, 254 - 8 bits.

| Decimal | Binary   |
| ------- | -------- |
| 192     | 11000000 |
| 168     | 10101000 |
| 1       | 0000001  |
| 254     | 11111110 |

These are pretty hard to understand for humans, that is why IP addresses are written using the dotted decimal, because there are four decimal numbers, 192, 168, 1, 254, separated by dots.

## Network, host portion

{% hint style="info" %}
<mark style="color:yellow;">**192.168.1**</mark>**.**<mark style="color:purple;">**254**</mark><mark style="color:red;">**/24**</mark>
{% endhint %}

That <mark style="color:red;">`/24`</mark>  is used to identify which part of the IP address represents the network and which represents the end host. Since the IP address upside is 32 bits, it means that the first 24 bits of this IP address represent the <mark style="color:red;">network portion</mark> of the address, and the remaining 8 bits represent the <mark style="color:red;">end host portion.</mark>

## IPv4 Address Classes

<table><thead><tr><th>Class</th><th width="222">First octet</th><th>First octet numeric range</th><th>Prefix length</th></tr></thead><tbody><tr><td>A</td><td>0xxxxxxx</td><td>0-127</td><td>/8</td></tr><tr><td>B</td><td>10xxxxxx</td><td>128-191</td><td>/16</td></tr><tr><td>C</td><td>110xxxxx</td><td>192-223</td><td>/24</td></tr><tr><td>D</td><td>1110xxxx</td><td>224-239</td><td></td></tr><tr><td>E</td><td>1111xxxx</td><td>240-255</td><td></td></tr></tbody></table>

* Addresses in class D are reserved for 'multicast' addresses.
* Class E addresses are reserved for experimental uses.
* The end of the class A range is usually considered to be 126, NOT 127, and that is because of loopback addresses.

| Class   | Leading bits | Size of network number bit field | Size of rest bit field | Number of networks | Addresses per network |
| ------- | ------------ | -------------------------------- | ---------------------- | ------------------ | --------------------- |
| Class A | 0            | 8                                | 24                     | 128 (2^7)          | 16,777,216 (2^24)     |
| Class B | 10           | 16                               | 16                     | 16,384 (2^14)      | 65,536 (2^16)         |
| Class C | 110          | 24                               | 8                      | 2,097,152 (2^21)   | 256 (2^8)             |

* The first address in each network is the network address, it cannot be assigned to hosts.
* The last address of the network is the broadcast address, the Layer 3 address used when you want to send traffic to all hosts, and it cannot be assigned to hosts. So the host count is two less.&#x20;

## Loopback Addresses

* Address range: 127.0.0.0 - 127.255.255.255
* These addresses are used to test the 'network stack' (OSI, TCP/IP) on the local device.

{% hint style="info" %}
Try to ping any IP addresses from that range. The PC responds with the pings
{% endhint %}

## Netmask

A netmask is written in dotted decimal like an IP address, where the network portion is all 1s and the host portion is all 0s. For example, the network mask of a class A address is 255.0.0.0.  The network mask of class B is 255.255.0.0 and for the class C is 255.255.255.0.&#x20;

| Class | CIDR Notation | Subnet Mask   | Binary Representation                 |
| ----- | ------------- | ------------- | ------------------------------------- |
| A     | /8            | 255.0.0.0     | (11111111 00000000 00000000 00000000) |
| B     | /16           | 255.255.0.0   | (11111111 11111111 00000000 00000000) |
| C     | /24           | 255.255.255.0 | (11111111 11111111 11111111 00000000) |

## Network Address

* If the host portion of the address is all 0's, it means it is the <mark style="color:red;">network address</mark>, the identifier of the network itself.
* The network address CANNOT be assigned to a host.

## Broadcast Address

* The LAST address in a network, with a host portion of all 1's, is the <mark style="color:red;">broadcast address</mark> for the network.
* Like the network address, the broadcast address CANNOT be assigned to a host.&#x20;

## Maximum hosts per network

{% hint style="info" %}
<mark style="color:orange;">192.168.1</mark><mark style="color:blue;">.0</mark><mark style="background-color:orange;">/24</mark>
{% endhint %}

* This is a Class C network 192.168.1.0/24. Because it is class C, it uses a /24 prefix length, and therefore the last octet, the last 8 bits, are the host portions.&#x20;
* That means that the host portion can be 0 to 255. So 0, to 255 gives us a total of 256 addresses, which is $$2^8$$, because there are 8 bits. But, remember those two special address, if the host portion is all 0, it represents the network address (network id), and if the host portion is all 1, it represents the broadcast address.
* Those 2 addresses cannot be assigned to a host, so actually the maximum hosts per network is $$2^8 - 2$$, which is 254 for a class C network.

{% hint style="info" %}
<mark style="color:orange;">172.16</mark><mark style="color:blue;">.0.0</mark><mark style="background-color:orange;">/16</mark>
{% endhint %}

* This is a Class B network 172.16.0.0/16. It goes from 172.16.0.0/16 through 172.16.255.255/16. The host portion is 16 bites, giving us 65,536 possible addresses. There are two addresses that cannot be hosts, the network address and the broadcast address, so actually there are 65,534 maximum hosts per class B network.

This goes to same to the other classes. The formula for this is:

$$
2^n - 2
$$

## First / Last Usable Address

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

## Commands

* You need to be in privileged exec mode.

```bash
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0    unassigned      YES unset  administratively down down
GigabitEthernet0/1    unassigned      YES unset  administratively down down
GigabitEthernet0/2    unassigned      YES unset  administratively down down
GigabitEthernet0/3    unassigned      YES unset  administratively down down
R1#
```

* The status command is linked to the Layer 1 status of the interface.
* administratively down: Interface has been disabled with the 'shutdown' command.
* This is the default Status of Cisco router interfaces.
* Cisco switch interfaces are NOT administratively down by default.
* The protocol is the Layer 2 status. Because the interfaces are down at Layer 1, Layer 2 cannot operate, so all of these interfaces are down at Layer 2.

```bash
R1#show interfaces g0/0
GigabitEthernet0/0 is up, line protocol is up
  Hardware is iGbE, address is 0c1b.8444.f000 (bia 0c1b.8444.f000)
  Internet address is 10.255.255.254/8
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Auto Duplex, Auto Speed, link type is auto, media type is RJ45
  output flow-control is unsupported, input flow-control is unsupported
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:06, output 00:00:05, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     167 packets input, 30159 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 0 multicast, 0 pause input
     350 packets output, 39097 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
     105 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     1 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
```

```bash
R1#show interfaces description
Interface              Status         Protocol Description
Gi0/0                 up             up
Gi0/1                 up             up
Gi0/2                 up             up
Gi0/3                 admin down     down
```

## IPv4 Address Configuration

```bash
R1#conf t
Enter configuration commands, one per line. End with CNTL/Z.
R1(config)#interface gigabitethernet 0/0
R1(config-if)#
```

* To configure the interface itself, I have to enter interface config mode. So I use the command 'interface' followed by the name of the interface.

```bash
R1(config-if)#ip address 10.255.255.254 ?
  A.B.C.D  IP subnet mask

R1(config-if)#ip address 10.255.255.254 255.0.0.0
R1(config-if)#no shutdown
R1(config-if)#
*Dec  7 08:29:08.937: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
*Dec  7 08:29:09.938: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
R1(config-if)#
```

* By doing this you can set the IP address.&#x20;
* The no shutdown command is used to enable the interface.

