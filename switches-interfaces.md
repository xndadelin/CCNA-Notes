---
icon: arrow-up-arrow-down
---

# Switches Interfaces

## Network topology

<figure><img src=".gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

This is a single LAN, 192.168.1.0/24, witch one router, R1, two switches, SW1 and SW2 and for PCs, PC1, PC2, PC2 and PC4.

## Commands

{% hint style="info" %}
To see the interfaces of the switch, you use this command: `show ip interface brief`
{% endhint %}

```bash
SW1#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol
Vlan1                  unassigned      YES unset  up                    up
FastEthernet0/1        unassigned      YES unset  up                    up
FastEthernet0/2        unassigned      YES unset  up                    up
FastEthernet0/3        unassigned      YES unset  up                    up
FastEthernet0/4        unassigned      YES unset  up                    up
FastEthernet0/5        unassigned      YES unset  down                  down
FastEthernet0/6        unassigned      YES unset  down                  down
FastEthernet0/7        unassigned      YES unset  down                  down
FastEthernet0/8        unassigned      YES unset  down                  down
FastEthernet0/9        unassigned      YES unset  down                  down
FastEthernet0/10       unassigned      YES unset  down                  down 
FastEthernet0/11       unassigned      YES unset  down                  down 
FastEthernet0/12       unassigned      YES unset  down                  down 
```

* Router interfaces have the shutdown command applied by default = will be in the administratively down/down state by default
* Switch interfaces do NOT have the 'shutdown' command applied by default = will be in the up/up state if connected to another device OR in the down/down state if not connected to another device

{% hint style="info" %}
To see the status of the interfaces, use the command: `show interfaces status`
{% endhint %}

```bash
SW1#show interfaces status
Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1                        connected    1          a-full  a-100 10/100BaseTX
Fa0/2                        connected    trunk      a-full  a-100 10/100BaseTX
Fa0/3                        connected    1          a-full  a-100 10/100BaseTX
Fa0/4                        connected    1          a-full  a-100 10/100BaseTX
Fa0/5                        notconnect   1          auto    auto  10/100BaseTX
Fa0/6                        notconnect   1          auto    auto  10/100BaseTX
Fa0/7                        notconnect   1          auto    auto  10/100BaseTX
Fa0/8                        notconnect   1          auto    auto  10/100BaseTX
Fa0/9                        notconnect   1          auto    auto  10/100BaseTX
Fa0/10                       notconnect   1          auto    auto  10/100BaseTX
Fa0/11                       notconnect   1          auto    auto  10/100BaseTX
Fa0/12                       notconnect   1          auto    auto  10/100BaseTX
```

Auto means they are able to negotiate with the device they are connected to and use the fastest speed both devices are capable of.

{% hint style="info" %}
Configuring interface speed and duplex
{% endhint %}

```bash
SW1#conf t
Enter configuration commands, one per line. End with CNTL/Z.
SW1(config)#int f0/1
SW1(config-if)#speed ?
  10    Force 10 Mbps operation
  100   Force 100 Mbps operation
  auto  Enable AUTO speed configuration
SW1(config-if)#speed 100
SW1(config-if)#duplex ?
  auto  Enable AUTO duplex configuration
  full  Force full duplex operation
  half  Force half-duplex operation
SW1(config-if)#duplex full
```

{% hint style="info" %}
Configure interfaces based on a range
{% endhint %}

```bash
SW1(config)#interface range f0/5 - 12
SW1(config-if-range)#description ## not in use ##
SW1(config-if-range)#shutdown
00:42:36: %LINK-5-CHANGED: Interface FastEthernet0/5, changed state to administratively down
00:42:36: %LINK-5-CHANGED: Interface FastEthernet0/6, changed state to administratively down
00:42:36: %LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down
00:42:36: %LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down
00:42:36: %LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down
00:42:36: %LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down
00:42:36: %LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down
00:42:36: %LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down
SW1(config-if-range)#
```

```bash
SW1(config)#int range f0/5 - 6, f0/9 - 12
SW1(config-if-range)#no shut
00:57:07: %LINK-3-UPDOWN: Interface FastEthernet0/5, changed state to up
00:57:07: %LINK-3-UPDOWN: Interface FastEthernet0/6, changed state to up
00:57:07: %LINK-3-UPDOWN: Interface FastEthernet0/9, changed state to up
00:57:07: %LINK-3-UPDOWN: Interface FastEthernet0/10, changed state to up
00:57:07: %LINK-3-UPDOWN: Interface FastEthernet0/11, changed state to up
00:57:07: %LINK-3-UPDOWN: Interface FastEthernet0/12, changed state to up
```

## Full/Half Duplex&#x20;

* Half Duplex means that the device cannot send and receive data at the same time. If it is receiving a frame, it must wait before sending a frame.
* Full Duplex means that the device can send and receive data at the same time. It does not have to wait.

In modern networks that use switches, all devices can use full duplex on their interfaces.

## Ethernet hubs

This device was around before the network switch. The hub is much simpler than a switch, in fact it is simply a repeater.

* Any frame it receives, it floods like a switch does with a broadcast or unknown unicast frame.

<figure><img src=".gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

* If PC1 wants to send a frame to PC2, it will pass through the Ethernet Hub and flood the frame to PC2 and PC3.&#x20;
* PC3 will not receive it, because the destination MAC address of the frame is not the same as the PC3 MAC address.&#x20;
* PC2 will receive it, because the destination MAC address of the frame is the same as the PC2 MAC address.

<figure><img src=".gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

If 2 PCs try to send frames at the same time, the hub will not send one first and then send the other after, it simply tries to flood both at the same time, and this will result in a collision on the interface, and PC2 will not receive either frame intact.&#x20;

* All devices connected to a hub are part of what is called a collision domain. The frames they send could collide with frames any of the other devices connected to the hub send. To deal with collisions in a half-duplex situation like this, Ethernet devices uses a mechanism called 'CSMA/CD'.

## CSMA/CD

{% hint style="info" %}
This stands for Carrier Sense Multiple Access with Collision Detection
{% endhint %}

It describes how devices avoid collisions in a half-duplex situation, and how they react if collisions do occur.

* Before sending frames, devices listen to the collision domain until they detect that other devices are not sending frames.
* If a collision does occur, the device sends a jamming signal to inform the other devices that a collision happen.&#x20;
* Each device will wait a random period of time before sending frames again.
* The process repeats.

<figure><img src=".gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

## Collision domains

Switches are more sophisticated than hubs. Hubs are simple repeaters which operate at Layer 1, repeating whatever signal they receive. Switches operate at Layer 2, using Layer 2 addressing, MAC addresses, to send frames to specific hosts. They also will not try to send two frames to the same host at once. So this network, which was one collision domain when connected to a hub, is now a triple collision domain.

<figure><img src=".gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

* Because of the improved functionality of switches over hubs, these devices can now operate in full duplex, meaning they do not have to worry about whether or not other devices are sending data at the same time, they can send data freely.&#x20;
* Although problems like collisions still do occur, they are rare and usually are a sign of a problem, like a misconfiguration, rather than a regular occurrence like in a half-duplex network.

## Speed/Duplex Autonegotiation

Interfaces that can run at different speeds (10/100 or 10/100/1000) have default settings of speed auto and duplex auto.

* Interfaces advertise their capabilities to the neighboring device, and they negotiate the best speed and duplex settings they are both capable of.

If autonegotiation is disabled on the device, this is how the switch will respond:

* SPEED&#x20;
  * The switch will try to sense the speed that the other device is operating at. If it fails to sense the speed, it will use the slowest supported speed.
* DUPLEX
  * If the speed is 10 or 100 Mbps, the switch will use half duplex.
  * If the speed is 1000Mbps or greater, use full duplex.

## Interface Errors

| Error         | Definition                                                      |
| ------------- | --------------------------------------------------------------- |
| Runts         | Frames that are smaller than the minimum frame size. (64 bytes) |
| Giants        | Frames that are larger than the maximum frame size (1518 bytes) |
| CRC           | Frames that failed the CRC check (in the Ethernet FCS trailer)  |
| Frame         | Frames that have an incorrect format (due to an error)          |
| Input errors  | Total of various counters, such as the above four               |
| Output errors | Frames the switch tried to send, but failed due to an error     |





&#x20;
