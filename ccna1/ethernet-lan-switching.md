---
description: >-
  LAN it's a network contained withing a relatively small area, like an office
  floor, or your home network. Routers are used to separate LANs.
icon: circle-nodes
---

# Ethernet LAN Switching

## Ethernet Frame

* There Ethernet header has 5 fields:
  * Preamble and SFD (Start Frame Delimiter). These are used for synchronization and to allow the receiving device to be prepared to receive the data in the frame.
  * Destination, the Layer 2 address to which the frame is being sent.
  * Source, the Layer 2 address of the device which sent the frame.
  * Type, it indicates the Layer 3 protocol used in encapsulated Packet, which is almost always IPv4 or IPv6. However, sometimes this a length field, indicating the length of the encapsulated  data, depending on the version of the Internet.
* The Ethernet trailer has only one field:
  * FCS (Frame Check Sequence). It's used by the receiving device to detect any errors that might have occurred in transmission.

### Preamble & SFD

* Preamble
  * It has a length of 7 bytes.
  * The purpose of this is that it allows devices to synchronize their receiver clocks, to make sure they are ready to receive the rest of the frame and the data inside.
* SFD - Start Frame Delimiter
  * It has a length of 1 byte.
  * It indicates the end of the preamble and the beginning of the rest of the frame.

Those two usually not considered part of the Ethernet header, although it is sent with every Ethernet frame. Therefore the size of the Ethernet header + trailer is 18 bytes.

* There is a minimum size for an Ethernet frame, which is 64 bytes (Header + Packet + Trailer).&#x20;
* 64 bytes - 18 bytes (header + trailer size) = 46 bytes, therefore there minimum packet size is 46 bytes.
* If the packet is less than 46 bytes, padding bytes are added.

### Destination & Source

* They indicate the devices sending and receiving the frame.
* Consist of the destination and source 'MAC address'.
* MAC = Media Access Control, is a 6 byte address of the physical device. It is assigned to the device when it's made.

### Type / Length

* It has a length of 2 bytes.
* It can be used to represent either the type of the encapsulated packet, or the length of the encapsulated packet.&#x20;
* A value of 1500 or less in this field indicates the LENGTH of the encapsulated packet (in bytes)
* A value of 1536 or greater in this field indicated the TYPE of the encapsulated packet (usually IPv4 or IPv6), and the length is determined via other methods.
  * IPv4 = 0X0800 = 2048 in decimal, which indicates the type ,
  * IPv6 = 0X86DD = 34525 in decimal, which indicates the type.

### FCS

* Stands for Frame Check Sequence.
* It has a length of 4 bytes.
* It's purpose is to detect corrupted data by running a 'CRC' algorithm over the received data.
* CRC = 'Cyclic Redundancy Check'

## MAC Address

* Is a 6 bytes physical address assigned to the device when it is made.
* A.K.A 'Burned-In Address' (BIA)
* Is globally unique, although there are MAC addresses known as 'locally-unique' MAC address, which do not have to be globally unique throughout the wold, however in almost all cases MAC addresses are globally unique.
* The first 3 bytes of the MAC address are the OUI (Organizationally Unique Identifier), which is assigned to the company unique device.
* The last 3 bytes are unique to the device itself.
* MAC addresses are written as a series of 12 hexadecimal characters.

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

* So here’s a simple network, just three PCs connected to a switch. Notice the interface names for the switch, F0/1, F0/2, and F0/3.
* F means Fast Ethernet, so these are 100 megabit per second interfaces. I’ve also written the MAC address for each PC. Notice each MAC address is a series of 12 hexadecimal digits, separated by periods. You may also see periods after every other digit, so for example PC1’s MAC address would be AA dot AA dot AA dot 00 dot 00 dot 01. But I tend to write them after every fourth character.
* The OUI, or organizationally unique identifier, which is the first half of the MAC address, is AAAAAA for each device, so we know that these PCs are all from the same maker. The second half of the MAC address of each device, however, is different for each PC, as the second half identifies the device itself. Now, let’s say PC1 wants to send some data to PC2. Due to lack of space I’ve just written an abbreviated form of the destination and source MAC addresses here. By the way, this kind of frame is called a ‘unicast frame’, a frame destined for a single target, PC2 in this case.&#x20;
* PC1 sends the frame through it’s network interface card, which is connected to SW1, and SW1 receives the frame. After SW1 receives the frame, it looks at the source MAC address field of the frame and then uses that information to LEARN where PC1 is. As you can see here, it adds the MAC address AAAA.AA00.0001 to it’s MAC Address table, and it associates that MAC address with its F0/1 interface.&#x20;
* This is known as a ‘dynamically learned’ MAC address, or just ‘dynamic MAC address’, because it wasn’t manually configured on the switch, the switch learned it itself. Every switch will keep a MAC address table like this, and they fill the MAC address table dynamically by looking at the source MAC address of frames it receives. Since SW1 received a frame from source MAC Address AAAA.AA00.0001 on it’s F0/1 interface, it knows that I can reach that MAC address on that interface, and adds it to the MAC address table.&#x20;
* This is how switches dynamically learn where each device on the network is, by looking at the source MAC address of the frame. Now, there is one problem. The destination of the frame is AAAA.AA00.0002, but SW1 doesn’t know where that is. This, by the way, is called an ‘unknown unicast’ frame, a frame for which the switch doesn’t have an entry in its MAC Address table.&#x20;
* Because the switch doesn’t know how to reach the destination, it has only one option. That is to <mark style="color:yellow;">FLOOD</mark> the frame.
* Flood means to forward the frame out of ALL of its interfaces, except the one it received the packet on. SW1 copies the frame and sends it out its F0/2 and F0/3 interfaces. It doesn’t send it out of its F0/1 interface, because that’s the interface it received the frame on. PC3 ignores the packet, because the destination MAC address doesn’t match its own MAC address, it simply drops the packet. PC2, however, receives the packet, and then processes it normally, up the OSI stack.

<mark style="color:yellow;">**Unicast frame**</mark>: a frame destined for a single target.&#x20;

<mark style="color:yellow;">**Unknown Unicast**</mark> frame: a frame for which the switch doesn't have an entry in its MAC Address Table.

<mark style="color:yellow;">**Known Unicast frame**</mark>: a frame for which the switch has already registered the MAC Address in the table.

* Dynamic MAC addresses are removed from the MAC address table after 5 minutes of inactivity.
* You can also manually remove MAC addresses from the table

## Ethernet LAN Switching

* Encapsulated within that frame is an Internet Protocol, known as IP packet, and that IP packet includes a source and destination IP address.

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

* For example, if PC1 wants to send data to PC3, the source IP will be 192.168.1.1 and the destination IP will be 192.168.1.3. The source MAC will be 0C2F.B011.9D00, however PC1 doesn’t actually know PC3’s MAC address.
* When you send data to another computer, you enter the IP address, not the MAC address. So, the user entered the IP address 192.168.1.3 as the destination, but PC1 has to discover PC3’s MAC address by itself. Remember, these switches are Layer 2 devices, they don’t operate at Layer 3, so they need to use MAC addresses, not IP addresses.&#x20;
* So, PC1 wants to send this Ethernet frame  to PC3, but first it has to learn PC3’s MAC address. To do so, it uses something called ARP, the Address Resolution Protocol.

### ARP&#x20;

{% hint style="info" %}
ARP - Address Resolution Protocol
{% endhint %}

* It is used to discover the Layer 2 address (MAC address) of a known Layer 3 address (IP address).
* Consists of two messages:
  * ARP Request (sent by the device who wants to know the MAC address of the other device)
  * ARP Reply (which is sent to inform the requesting device of the MAC address)
* ARP Request is broadcast = sent to all hosts on the network, because the Layer 2 address of the destination host is unknown, it broadcasts the request and waits for a reply from the correct device.
* ARP Reply is unicast = sent only to one host (the host that sent the request)
* The ARP type is: OX0806

#### ARP Table

* Use this command to view the ARP table

```bash
arp -a
```

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

* Internet Adresss = IP address (Layer 3 address)
* Physical Address = MAC address (Layer 2 address)
* Type static = default entry
* Type dynamic = learned via ARP

### Ping

* Network utility that is used to test reachability.
* Measures the round-trip time.
* Uses two messages:
  * ICMP(Internet Control Message Protocol) Echo Request&#x20;
  * ICMP(Internet Control Message Protocol) Echo Reply
* The command to use ping: ping  <mark style="background-color:yellow;">\<ip-address></mark>

### Commands

* Use this command to show the MAC address table:

```bash
SW1#show mac address-table
```

* Use this command to clear the MAC address table:

```bash
SW1#clear mac address-table dynamic
```

* If you do not want to clear all the MAC addresses from the table, you can add some additional options to the command:

```bash
SW1#clear mac address-table dynamic address [MAC address]
```

* You can also clear the MAC addresses table based on the interfaces:

```bash
SW1#clear mac address-table dynamic interface [interface-id]
```
