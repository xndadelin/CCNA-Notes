---
icon: trees
---

# Rapid STP

## Spanning Tree Versions

| IEEE Standards                           | Description                                                                                                                                                                    |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Spanning Tree Protocol (802.1D)          | <p>- The original STP<br>- All VLANs share one STP instance<br>- Therefore, cannot load balance</p>                                                                            |
| Rapid Spanning Tree Protocol (802.1w)    | <p>- Much faster at converging/adapating to network changes than 802.1D<br>- All VLANs share one STP instance<br>- Therefore, cannot load balance</p>                          |
| Multiple Spanning Tree Protocol (802.1s) | <p>- Uses modified RSTP mechanics<br>- Can group multiple VLANs into different instances (ie. VLANs 1-5 in instance 1, VLANs 6-10 in instance 2) to perform load balancing</p> |

***



| Cisco Versions                                  | Description                                                                                                                               |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Per-VLAN Spanning Tree Plus (PVST+)             | <p>- Cisco's upgrade to 802.1D<br>- Each VLAN has its own STP instance<br>- Can load balance by blocking different ports in each VLAN</p> |
| Rapid Per-VLAN Spanning Tree Plus (Rapid PVST+) | <p>- Cisco's upgrade to 802.1w<br>- Each VLAN has its own STP instance<br>- Can load balance by blocking different ports in each VLAN</p> |

## Rapid Spanning Tree Protocol

{% hint style="info" %}
RSTP is not a timer-based spanning tree algorithm like 802.1D. Therefore, RSTP offers an improvement over the 30 seconds or more that 802.1D takes to move a link to forwarding. The heart of the protocol is a new bridge-bridge handshake mechanism, which allows ports to move directly to forwarding.
{% endhint %}

Similarities between STP and RSTP:

* RSTP serves the same purpose as STP, blocking specific ports to prevent Layer 2 loops.&#x20;
* RSTP elects a root bridge with the same rules as STP.
* RSTP elects root ports with the same rules as STP.
* RSTP elects designated ports with the same rules as STP.

| Speed    | STP Cost | RSTP Cost |
| -------- | -------- | --------- |
| 10 Mbps  | 100      | 2,000,000 |
| 100 Mbps | 19       | 200,000   |
| 1 Gbps   | 4        | 20,000    |
| 10 Gbps  | 2        | 2,000     |
| 100 Gbps | X        | 200       |
| 1 Tbps   | X        | 20        |

The Blocking, Listening and Disabled are now combined into a single state called **DISCARDING.**

| STP Port State | Send/Receive BPDUs | Frame forwarding (regular traffic) | MAC address learning | Stable/ Transitional |
| -------------- | ------------------ | ---------------------------------- | -------------------- | -------------------- |
| **Discarding** | NO/YES             | NO                                 | NO                   | Stable               |
| **Learning**   | YES/YES            | NO                                 | YES                  | Transitional         |
| **Forwarding** | YES/YES            | YES                                | YES                  | Stable               |

* If a port is administratively disabled (shutdown command) = discarding state.
* If a port is enabled, but blocking traffic to prevent Layer 2 loops = discarding state.

## Port Roles

* The root port role remains unchanged in RSTP.
  * The port that is closest to the root bridge becomes the root port for the switch.
    * The root bridge is the only switch that does not have a root port.
* The designated port role also remains unchanged in RSTP.
  * The port on a segment (collision domain) that sends the best BPDU is that segment's designated port (only one per segment).
* The non-designated port is split into two separate roles in RSTP:
  * Alternate port role
  * Backup port role

### RSTP: Alternate port role

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

* Discarding port that receives a superior BPDU from another switch.
* Functions as a backup to the root port.
* If the root port fails, the switch can immediately move its best alternate port to forwarding.
  * This immediate move to forwarding state functions like a classic STP optional feature called UplinkFast. Beacuse it is build into RSTP, you do not need to activate UpLink when using RSTP/Rapid PVST+.

### BackboneFast functionality

* One more STP optional feature that was built into RSTP is **BackboneFast.**

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

* BackboneFast allows SW3 to expire the max age timers on its interface and rapidly forward the superior BPDUs to SW2.

### RSTP: Backup port role

* Discarding port that receives a superior BPDU from **another interface on the same switch**.

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

* This only happend when two interfaces are connected to the same collision domain (via a hub).
* Function as a backup for a designated port.
* The interface with the lowest port ID will be selected as the designated port, and the other will be the backup port.&#x20;

Rapid STP is compatible with Classic STP. The interface(s) on the Rapid STP-enabled switch connected to the Classic STP-enabled switch will operate in Classic STP mode (timers, blocking > listening > learning > forwarding process, etc).

In classic STP, only the root bridge originated BPDUs are being forwarded. In rapid STP, ALL switches originate and send their own BPDUs from their designated ports.

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

* All switches running Rapid STP send their own BPDUs every hello time (2 seconds).
* Switches ‘age’ the BPDU information much more quickly. In classic STP, a switch waits 10 hello intervals (20 seconds). In rapid STP, a switch considers a neighbor lost if it misses 3 BPDUs (6 seconds). It will then ‘flush’ all MAC addresses learned on that interface.

## RSTP Link Types

* RSTP distinguishes between three different ‘link types’.
  * **Edge**: a port that is connected to an end host. Moves directly to forwarding, without negotiation.
  * **Point-to-point**: a direct connection between two switches.
  * **Shared**: a connection to a hub. Must operate in half-duplex mode.

### Edge

* Edge ports are connected to end hosts.
* Because there is no risk of creating a loop, they can move straight to the forwarding state without the negotiation process.
* They function like a classic STP port with PortFast enabled.

```
SW1(config-if)#spanning-tree porfast
```

### Point to point

* Point-to-point ports connect directly to another switch.
* They function in full-duplex.
* You don't need to configure the interface as point-to-point (it should be detected).

```
SW1(config-if)# spanning-tree link-type point-to-point
```

### Shared&#x20;

* Shared ports connect to another switch (or switches) via a hub.
* They function in half-duplex.
* You don't need to configure the interface as shared (it should be detected).

```
SW1(config-if)# spanning-tree link-type shared
```
