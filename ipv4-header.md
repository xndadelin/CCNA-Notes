---
icon: h4
---

# IPv4 Header

<figure><img src=".gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

## Version field

{% hint style="info" %}
Length of 4 bits
{% endhint %}

* It identifies the version of IP used, which are:

| Version | IP   | Decimal |
| ------- | ---- | ------- |
| 0100    | IPv4 | 4       |
| 0110    | IPv6 | 6       |

## IHL - Internet Header Length

{% hint style="info" %}
Length of 4 bits
{% endhint %}

* The final field of the IPv4 header (Options) is variable in length, so this field is necessary to indicate the total length of the header.&#x20;
* It identifies the length of the header in <mark style="color:purple;">4-byte increments</mark><mark style="color:blue;">.</mark>

e.g:

> The IHL is 5 which means that the length of the header is 5 x 4 = 20 bytes

* The minimum value is 5 (=20 bytes). That is the length of the IPv4 header without any IP options at the end.
* The maximum value is 15 (15 x 4 = 60 bytes).

## DSCP - Differentiated Services Code Point

{% hint style="info" %}
Length of 6 bits
{% endhint %}

* It is used for QoS (Quality of Service)
* It is used to prioritize delay-sensitive data (streaming voice, video, etc.)

## ECN - Explicit Congestion Notification

{% hint style="info" %}
Length of 2 bits
{% endhint %}

* Provides end-to-end (between two endpoints) notification of network congestion without dropping packets.
* It's a optional feature that requires both endpoints, as well as the underlying network infrastructure, to support it.

## Total Length

{% hint style="info" %}
Length is 16 bits
{% endhint %}

* Indicates the total length of the packet (L3 header + L4 segment)
* Measured in bytes (not 4-byte increments like IHL)
* Minimum value of 20 (=IPv4 header with no encapsulated data)
* Maximum value is 65,535 (maximum 16-bit value)

## Identification

{% hint style="info" %}
Length is 16 bits
{% endhint %}

* If a packet is fragmented due to being too large, this field is used to identify which packet the fragment belongs to.
* All fragments of the same packet will have their own IPv4 header with the same value in this field.
* Packets are fragmented if larger than the MTU (Maximum Transmission Unit).
* The MTU is usually 1500 bytes
* Maximum payload size of a Ethernet frame is 1500 bytes (these are related).
* Fragments are reassembled by the receiving host.

## Flags

{% hint style="info" %}
Length of 3 bits
{% endhint %}

* Used to control/identify fragments.
  * The first bit is 0: Reserved, always set to 0.
  * The second bit is "Dont Fragment" (DF bit), used to indicate a packet that should not be fragmented.
  * The third bit is "More Fragments" (MF bit), set to 1 if there are more fragments in the packet, set to 0 for the last fragment.

> Unfragmented packets will always have their MF bit set to 0.

## Fragment Offset

{% hint style="info" %}
13 bits in length
{% endhint %}

* Used to indicate the position of the fragment within the original, unfragmented IP packet.
* Allows fragmented packets to be reassembled even if the fragments arrive out of order.

## TTL - Time To Live

{% hint style="info" %}
Length is 8 bits
{% endhint %}

* A router will drop a packet with a TTL of 0.
* Used to prevent infinite loops.
* Originally designed to indicate the packet's maximum lifetime in seconds.
* In practice, it indicates a 'hop count':
  * each time the packet arrives at a router, the router decreases the TTL by 1.

> The recommended default TTL is 64.

## Protocol

{% hint style="info" %}
Length of 8 bits
{% endhint %}

* It indicates the protocol of the encapsulated Layer 4 PDU.
  * _<mark style="color:red;">**6**</mark>**&#x20;****- for TCP**_
  * _<mark style="color:red;">**17**</mark>**&#x20;****- for UDP**_
  * _<mark style="color:red;">**1**</mark>**&#x20;****- for ICMP**_
  * _<mark style="color:red;">**89**</mark>**&#x20;****- for OSPF (dynamic routing protocol)**_

## Header Checksum

{% hint style="info" %}
Length of 16 bits
{% endhint %}

* A calculated checksum used to check for errors in the IPv4 header.
* When a router receives a packet, it calculates the checksum of the header and compares it to the one in this field of the header.
* If they do not match, the router drops the packet.
* Only used to check for errors only in the IPv4 header.
  * IP relies on the encapsulated protocol to detect error in the encapsulated data.
  * Both TCP and UDP have their own checksum fields to detect errors in the encapsulated data.

## Source/Destination IP Address

{% hint style="info" %}
Length of 32 bits (each)
{% endhint %}

* The source IP address indicates the IPv4 address of the sender of the packet.
* The destination IP address indicated the IPv4 address of the intended receiver of the packet.

## Options

{% hint style="info" %}
Length varies from 0 to 320 bits.
{% endhint %}

* Rarely used.
* If the IHL field is greater than 5, it means that Options are present.





