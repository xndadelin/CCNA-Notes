---
icon: swap-arrows
---

# NAT

## Private IPv4 addresses (RFC 1918)

* IPv4 does not provide enough addresses for all devices that need an IP address in the modern world.&#x20;
* The long-term solution is to switch to IPv6
* There are three main short-term solutions:
  * CIDR
  * Private IPv4 adresses
  * NAT
* RFC 1918 specifies the following IPv4 address ranges as private:
  * 10.0.0.0/8
  * 172.16.0.0/12
  * 192.168.0.0/16

> Private IP addresses cannot be used over the internet!

## Network Address Translation (NAT)

* Network Address Translation is used to modify the source and/or destination IP addresses of packets.
* There are various reasons to use NAT, but the most common reason is to allow hosts with private IP addresses to communicate with other hosts over the internet.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Static NAT

* Static NAT involves statically configuring one-to-one mappings of private IPs to public IPs.
* An <mark style="color:red;">inside local IP</mark> address is mapped to an <mark style="color:red;">inside global IP</mark> address.
  * An inside local IP is the IP address of the inside host, from the perspective of the _local_ network.
  * An inside global IP is the IP address of the inside host, from the perspective of the _outside_ hosts.
* Static NAT allows devices with private IP addresses to communicate over the Internet. However, because it requires a one-to-one IP address mapping, it does not help preserve IP addresses.

<pre class="language-c"><code class="lang-c">!!Define the 'inside' interface(s) connected to the internal network
R1(config)#int g0/1
R1(config-if)#ip nat inside

!!Define the 'outside' interface(s) connected to the external network.

R1(config-if)#int g0/0
R1(config-if)#ip nat outside
R1(config-if)#exit

!!!Configure the one-to-one IP address mappings.
<strong>!!!ip nat inside source static inside-local-ip inside-global-ip
</strong>

R1(config)#ip nat inside source static 192.168.0.167 100.0.0.1
R1(config)#ip nat inside source static 192.168.0.168 100.0.0.2
R1(config)#exit

R1#show ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
udp 100.0.0.1:56310    192.168.0.167:56310 8.8.8.8:53        8.8.8.8:53
--- 100.0.0.1          192.168.0.167      ---                ---
udp 100.0.0.2:62321    192.168.0.168:62321 8.8.8.8:53        8.8.8.8:53
--- 100.0.0.2          192.168.0.168      ---                ---
</code></pre>

* The outside local address is the IP address of the outside host, from the perspective of the local network.
* The outside global address is the IP address of the outside host, from the perspective of the outside network.

> Inside/Outside = LOCATION OF THE HOST
>
> Local/Global = PERSPECTIVE

```c
R1#clear ip nat translation *
!THIS CLEARS ALL DYNAMIC TRANSLATIONS
```

```
R1#show ip nat statistics
```

## Dynamic NAT

* In dynamic NAT, the router dynamically maps inside local addresses to inside global addresses as needed.
* An ACL is used to identify which traffic should be translated.
  * If the source IP is permitted by the ACL, the source IP will be translated.
  * If the source IP is denied by the ACL, the source IP will NOT be translated.&#x20;
* A NAT pool is used to define the available inside global addresses.
* If there are not enough inside global IP addresses available, it is called 'NAT pool exhaustion'.

```c
R1(config)#int g0/1
R1(config-if)#ip nat inside

R1(config-if)#int g0/0
R1(config-if)#ip nat outside
R1(config-if)#exit

R1(config)#access-list 1 permit 192.168.0.0 0.0.0.255

R1(config)#ip nat pool POOL1 100.0.0.0 100.0.0.255 prefix-length 24

R1(config)#ip nat inside source list 1 pool POOL1
```

## PAT (Port Address Translation)

* PAT (aka NAT overload) translates both the IP address and the port number (if necessary)
* By using a unique port number for each communication flow, a single public IP address can be used by many different internal hosts.&#x20;
* Because many inside hosts can share a single public IP, PAT is very useful for preserving public IP addresses, and it is used in networks all over the world.

```c
R1(config)#int g0/1
R1(config-if)#ip nat inside

R1(config-if)#int g0/0
R1(config-if)#ip nat outside
R1(config-if)#exit

R1(config)#access-list 1 permit 192.168.0.0 0.0.0.255

R1(config)#ip nat pool POOL1 100.0.0.0 100.0.0.255 prefix-length 24

R1(config)#ip nat inside source list 1 pool POOL1 overload
```

```c
R1(config)#int g0/1
R1(config-if)#ip nat inside

R1(config-if)#int g0/0
R1(config-if)#ip nat outside
R1(config-if)#exit

R1(config)#access-list 1 permit 192.168.0.0 0.0.0.255
R1(config)#ip nat inside source list 1 interface g0/0 overload
```

```c
R1(config)# ip nat pool pool-name start-ip end-ip prefix-length prefix-length
R1(config)# ip nat pool pool-name start-ip end-ip netmask subnet-mask
R1(config)# ip nat inside source list access-list pool pool-name
R1(config)# ip nat inside source list access-list pool pool-name overload
R1(config)# ip nat inside source list access-list interface interface overload
```
