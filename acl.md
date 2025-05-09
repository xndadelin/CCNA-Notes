---
description: Access control lists control the access control to the network.
icon: list-radio
---

# ACL

## Introduction

* ACLs function as a packet filter, instructing the router to permit or discard specific traffic.
* ACLs can filter traffic based on source/destination IP addresses, source/destination Layer 4 ports, etc.

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

> Hosts in 192.168.1.0/24 should be able to access the 10.0.1.0/24 network.
>
> Hosts in the 192.168.2.0/24 should not be able to access the 10.0.1.0/24 network.&#x20;

* ACLs are configured globally on the router. (global config mode)
* They are an ordered sequence of ACEs. (Access Control Entries)

<figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

* Configuring an ACL in global config mode will not make the ACL take effect.
* The ACL must be applied to an interface.
* ACLs are applied either inbound or outbound.
* ACLs are made up of one or more ACEs.
* When the router checks a packet against the ACL, it processes the ACEs in order, from top to bottom.
* If the packet matches one of the ACEs in the ACL, the router takes the action and stops processing the ACL. All entries below the matching entry will be ignored.
* A maximum of one ACL can be applied to a single interface per direction.&#x20;

## Implicit deny

* What will happen if a packet does not match any of the entries in an ACL?
  * The router will deny the packet, not forward it.
* There is an implicit deny at the end of all ACLs.
* The implicit deny tell the router to deny all traffic that does not match any of the configured entries in the ACL.

## ACL Types

* Standard ACL : match based on Source IP address **only**
  * Standard Numbered ACLs
  * Standard Named ACLs
* Extended ACL: match based on Source/Destination IP, Source/Destination port, etc.
  * Extended Numbered ACLs
  * Extended Named ACLs

### Standard ACLs&#x20;

* Match traffic based only on the source IP address of the packet.
* Numbered ACLs are identified with a number (ie. ACL 1, ACL 2, etc).
* Different types of ACLs have a different range of numbers that can be used.
  * Standard ACLs can use 1-99 and 1300-1999.
* The basic command to configure a standard numbered ACL is:

```
R1(config)# access-list number { deny | permit } ip wildcard-mask
```

```c
R1(config)# access-list 1 deny 1.1.1.1 0.0.0.0
R1(config)# access-list 1 deny 1.1.1.1
R1(config)# access-list 1 deny host 1.1.1.1

R1(config)# access-list 1 permit any
R1(config)# access-list 1 permit 0.0.0.0 255.255.255.255

R1(config)# access-list 1 remark ## BLOCK ADELIN FROM ACCOUNTING ## 
```

```
! Apply an ACL to an interface
R1(config-if)# ip access-group number { in | out }
```

## Standard named ACL

* Match traffic based only on the source IP address of the packet.
* Named ACLs are identified with a name (ie. 'GIANT\_TREE')
* Standard named ACLs are configured by entering 'standard named ACL config mode', and then configuring each entry within that config mode.

```
R1(config)# ip access-list standard acl-name
R1(config-std-nacl)# [entry-number] {deny | permit} ip wildcard-mask
R1(config-if)# ip access-group GROUP_NAME { in | out }
```

* Standard ACLs should be applied as close to the destination as possible.

## Numbered ACLs with subcommands

```
R1(config)# ip access-list standard acl-number
R1(config-std-nacl)# [entry-number] {deny | permit} ip wildcard-mask
```

* You can easily delete individual entries in the ACL with `no [entry-number]`&#x20;

## Resequencing ACLs

* There is a resequencing function that helps edit ACLs.
* The command is \`
