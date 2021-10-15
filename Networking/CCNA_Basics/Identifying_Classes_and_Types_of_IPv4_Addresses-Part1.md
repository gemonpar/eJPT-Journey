#  Identifying Classes & Types of IPv4 Addresses :: Part 1 
---
An IPv4 address is 32-bit long, divide in four groups of 8 bits (octets).

### IP Bit Patterns
- **Multicast:** One-to-many communication.
- **Broadcast:** One-to-all communication.
- **Unicast:** One-to-One communication.

### 3 Classes of IPv4
- **Class A:** 0.0.0.0 - 127.255.255.255
- **Class B:** 128.0.0.0 - 191.255.255.255
- **Class C:** 192.0.0.0 - 223.255.255.255
- **Class D:** 224.0.0.0 - 239.255.255.255
- **Class E:** 240.0.0.0 - 255.255.255.255

**Note:** 127.x.x.x ranges are considered reserved for loopbacks.
**Note:** 169.254.x.x are considered as APIPA (Automatic Private Internet Protocol Addressing) when the host not founds an DHCP server in the network.

### Classes for Unicast (Classes A, B and C)

|Class|Network Bits|Host Bits|Usable Addresses|
|:-:|:-:|:-:|:-:|
|**A**|8|24|1.any.any.any - 126.any.any.any|
|**B**|16|16|128.any.any.any - 191.any.any.any|
|**C**|24|8|192.any.any.any - 223.any.any.any|

### Class for Multicast (Class D)

|Types|Range|Description|
|:-:|:-:|:-:|
|**1**|224.0.0.0 - 224.0.0.255|Reserved for special “well-known” multicast addresses|
|**2**|224.0.1.0 - 238.255.255.255|Globally-scoped (Internet-wide) multicast addresses|
|**3**|239.0.0.0 - 239.255.255.255|Administratively-scoped (local) multicast addresses|

### Class for experimental (Class E)

This class is for experimental purposes, so it's not meant to be used for usage of hosts IPs ranges.

### IPv4 Governing Bodies

There is a need to the existance of Governing Bodies in IPv4 in order to avoid IPs to be repeated.

There is the **IANA (Internet Assigned Numbers Authority)**, this one is divided in another 5 subodies named RIRs(Regional Internet Registries):

- **ARIN (American Registry for Internet Numbers)**
- **RIPE NCC (Réseaux IP Européens Network Coordination Centre)**
- **APNIC (The Asia-Pacific Network Information Centre)**
- **LACNIC (Latin American and Caribbean Internet Address Registry)**
- **AfriNIC (The African Network Information Centre)**

Under that level there are the **ISPs (Internet Service Providers)** that serves internet to the end users.
