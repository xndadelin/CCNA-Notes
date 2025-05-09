---
icon: apartment
---

# Domain Name System

* DNS is used to resolve human-readable names (such as google.com) to IP addresses.
* Machines such as PCs do not use names, they use addresses.
* Names are much easier for us to use and remember than IP addresses.
  * What is the IP address of `youtube.com`
* &#x20;When you type `youtube.com` into a web browser, your device will ask a DNS server for the IP address of youtube.com.
* The DNS server your device uses can be manually configured or learned via DHCP.

## Configuration

```
R1(config)#ip dns server

R1(config)#ip host R1 192.168.0.1
R1(config)#ip host PC1 192.168.0.101
R1(config)#ip host PC2 192.168.0.102
R1(config)#ip host PC3 192.168.0.103

R1(config)#ip name-server 8.8.8.8

R1(config)#ip domain lookup
```

```bash
R1(config)#do ping youtube.com
Translating "youtube.com"
% Unrecognized host or address, or protocol not running.
```

```bash
R1(config)#ip name-server 8.8.8.8
```

```bash
R1(config)#ip domain lookup
```

```bash
R1(config)#do ping youtube.com
Translating "youtube.com"...domain server (8.8.8.8) [OK]

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.217.25.110, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/10/13 ms
```

```bash
R1(config)#ip domain name jeremysitlab.com
```
