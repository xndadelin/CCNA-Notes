---
icon: battle-net
---

# Subnetting

Review the IPv4 **Address Classes**

{% content-ref url="ipv4-addressing.md" %}
[ipv4-addressing.md](ipv4-addressing.md)
{% endcontent-ref %}

IP addresses are assigned companies or organizations by a non-profit American corporation called the IANA, the Internet Assigned Numbers Authority.

* For example, a very large company might receive a class A or class B network, while a small company might receive a class C network
  * This led to many wasted IP addresses

The network between two directly connected routers is called a <mark style="color:red;">point-to-point network.</mark>&#x20;

Using the class C, which is the best option for now, we only need 4 addresses to configure the network in the point-to-point network. The class C requires 256 hosts, which is 252 more hosts than needed.

* Obviously, there are many IP addresses wasted.

Another example is: Company X needs IP addressing for 5000 end hosts.&#x20;

* A class C network does not provide enough addresses, so a class B network must be assigned.
* This will result in about 60000 addresses being wasted.

## CIDR -  Classless Inter-Domain Routing

When the internet was first created, the creators did not predict that the Internet would become as large as it is today. &#x20;

The IETF (Internet Engineering Task Force) introduced CIDR in 1993 to replace the classful addressing system.

With CIDR, the requirements of Class A, B and C were removed.

* This allowed larger networks to be split into smaller networks, allowing greater efficiency.
* These smaller networks are called 'subnetworks' or 'subnets'

## Number of usable addresses per subnet

$$
2^n - 2
$$

* n is the number of host bits

Configuring a point-to-point network requires a /31 mask, which means there are 0 usable addresses, but we do not need the broadcast and network address, which means we can configure the router's interfaces with the 2 IP addresses we can use.

| CIDR Notation | Subnet Mask     | Number of Addresses | Usable Hosts |
| ------------- | --------------- | ------------------- | ------------ |
| /32           | 255.255.255.255 | 1                   | 0            |
| /31           | 255.255.255.254 | 2                   | 2\*          |
| /30           | 255.255.255.252 | 4                   | 2            |
| /29           | 255.255.255.248 | 8                   | 6            |
| /28           | 255.255.255.240 | 16                  | 14           |
| /27           | 255.255.255.224 | 32                  | 30           |
| /26           | 255.255.255.192 | 64                  | 62           |
| /25           | 255.255.255.128 | 128                 | 126          |
| /24           | 255.255.255.0   | 256                 | 254          |
| /23           | 255.255.254.0   | 512                 | 510          |
| /22           | 255.255.252.0   | 1,024               | 1,022        |
| /21           | 255.255.248.0   | 2,048               | 2,046        |
| /20           | 255.255.240.0   | 4,096               | 4,094        |
| /19           | 255.255.224.0   | 8,192               | 8,190        |
| /18           | 255.255.192.0   | 16,384              | 16,382       |
| /17           | 255.255.128.0   | 32,768              | 32,766       |
| /16           | 255.255.0.0     | 65,536              | 65,534       |
| /15           | 255.254.0.0     | 131,072             | 131,070      |
| /14           | 255.252.0.0     | 262,144             | 262,142      |
| /13           | 255.248.0.0     | 524,288             | 524,286      |
| /12           | 255.240.0.0     | 1,048,576           | 1,048,574    |
| /11           | 255.224.0.0     | 2,097,152           | 2,097,150    |
| /10           | 255.192.0.0     | 4,194,304           | 4,194,302    |
| /9            | 255.128.0.0     | 8,388,608           | 8,388,606    |
| /8            | 255.0.0.0       | 16,777,216          | 16,777,214   |

\*️⃣ /31 is an exception and can be used for point-to-point links without reserved addresses for network and broadcast.

## Subnetting based on the number of subnets

$$
2^x
$$



* x = number of **borrowed** bits
* The formula express the number of subnets possible.

e.g

* Subnet the 192.168.255.0/24 network into five subnets of equal size.
  * In order to create 5 subnets, we need to borrow 3 bits from the host portion (because 5 <= 2^3)
  * The new mask is <mark style="color:yellow;">/24</mark> + the 3 bits we borrowed = <mark style="color:orange;">/27</mark>&#x20;
  * If we have a /27, it means we have 2^5 addresses possible for each subnet

1. <mark style="color:red;">192.168.255.0/27</mark>
2. <mark style="color:red;">192.168.255.32/27</mark>
3. <mark style="color:red;">192.168.255.64/27</mark>
4. <mark style="color:red;">192.168.255.96/27</mark>
5. <mark style="color:red;">192.168.255.128/27</mark>

## Identify the subnet

* What subnet does host 192.168.5.57/27 belong to?

In order to work out this, you need to make all the host bits 0 or make an AND bitwise operation with the mask.

## &#x20;Subnets/Hosts (Class C)

| Prefix Length | Number of Subnets | Number of Hosts |
| ------------- | ----------------- | --------------- |
| /25           | 2                 | 126             |
| /26           | 4                 | 62              |
| /27           | 8                 | 30              |
| /28           | 16                | 14              |
| /29           | 32                | 6               |
| /30           | 64                | 2               |
| /31           | 128               | 0 (2)           |
| /32           | 256               | 0 (1)           |

## Subnets/Hosts (Class B)

| Prefix Length | Number of Subnets | Number of Hosts |
| ------------- | ----------------- | --------------- |
| /17           | 2                 | 32766           |
| /18           | 4                 | 16382           |
| /19           | 8                 | 8190            |
| /20           | 16                | 4094            |
| /21           | 32                | 2044            |
| /22           | 64                | 1022            |
| /23           | 128               | 510             |
| /24           | 256               | 254             |
| /25           | 512               | 126             |
| /26           | 1024              | 62              |
| /27           | 2048              | 30              |
| /28           | 4096              | 14              |
| /29           | 8192              | 6               |
| /30           | 16384             | 2               |
| /31           | 32768             | 0 (2)           |
| /32           | 65536             | 0 (1)           |

## VLSM

**V**ariable-**L**ength **S**ubnet **M**ask is the process of creating subnets of different sizes, to make your use of network addresses more efficient.

<figure><img src=".gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

We have to subnet the **192.168.1.0/24** in order to accomodate all the LANs.

### Steps

1. Assign the largest subnet at start of the address space
2. Assign the second-largest subnet after it.
3. Repeat the process until all subnets have been assigned.

We have to sort in descending order the LANs.

1. Tokio LAN A - 110 hosts
2. Toronto LAN B - 45 hosts
3. Toronto LAN A - 29 hosts
4. Tokio LAN B - 8 hosts
5. The point-to point connection - (2 addreses, we can use a /31)

110 + 2 <= 2^7 ⇒ Mask: /32-7 = /25 (255.255.255.128)

* Network:  192.168.1.0/25
* Broadcast: 192.168.1.127/25&#x20;
* Number of hosts 2^(32 - 25) - 2 = 126

45 + 2<= 2^6 ⇒ Mask: /32 - 6 = /26 (255.255.255.192)

* Network: 192.168.1.128/26
* Broadcast: 192.168.191/26
* Number of hosts: 2^(32- 26) - 2 = 62

29 + 2 <= 2^5 ⇒ Mask: /32 - 5 = /27 (255.255.255.224)

* Network: 192.168.1.192/27
* Broadcast: 192.168.1.223/27
* Number of hosts: 2^(32 - 27) - 2 = 30

8 + 2 <= 2^4 ⇒ Mask: /32 - 4 = /28 (255.255.255.240)

* Network: 192.168.1.224/28
* Broadcast: 192.168.239/28
* Number of hosts: 2^(32-28) - 2 = 14

2 <= 2^1 ⇒ Mask: /32 - 1 = /31 (255.255.255.254)

* First address: 192.168.240.0/31
* Second address: 192.168.241.1/31

