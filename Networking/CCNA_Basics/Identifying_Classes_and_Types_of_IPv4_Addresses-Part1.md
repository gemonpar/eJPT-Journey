#  Identifying Classes & Types of IPv4 Addresses :: Part 1 
---
An IPv4 address is 32-bit long, divide in four groups of 8 bits (octets).

### IP Bit Patterns
- **Multicast:** One-to-many communication.
- **Broadcast:** One-to-all communication.
- **Unicast:** One-to-One communication.

### Classes of IPv4
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

### Private IPv4 Addresses
This IPs are used for internal use only in networks owned by companies or home networks.

##### Range of private address:
- **Class A:** 10.0.0.0 through 10.255.255.255
- **Class B:** 172.16.0.0 through 172.31.255.255
- **Class C:** 192.168.0.0 through 192.168.255.255

So when one device in a network tries to connect outside de local network, his private IP will be substituted with the public IP of the router. So the source ip will be the public ip of the router.

# IPv4 Subnet Masks 
---
In order to amplify the numbre of possible addresses in a network, when a company buy a private address from an ISP, the company is allowed to modify however he wants the host bits of the IP but cannot change the network bits.

Ex: 129.1.0.0 is a class B IP so the network bits are 16, however the company can make a subnet taking 2 bits more of the host so the company can make 99 subnets

Exercice: Gicen this IP, which is the subnet address and the broadcast of the 3rd subnet.
- 400 networks -> 2^9 = 512
- 130.130.192.0/18 -> 18+9=27 -> 255.255.255.224 -> 224 = 11100000 -> The third subnet it is 01000000 -> 130.130.192.64 it is the 3rd subnet address -> The broadcast address of the third subnet it is 01011111 -> 130.130.192.95.    

#  Same-Length Subnet Masking vs Variable-Length Subnet Masking
---
### Same-Length Subnet Masking
The problem with same-length subnetting mask is that when dividing a network into different subnets could be resulting in one network requiring more hosts than the amount of hosts available in a subnet. 
For example:

Starting network is **199.199.199.0/24**.
You requires **4 subnets** with the given specifications:
- **Network 1 - 5 hosts**
- **Network 2 - 80 hosts**
- **Network 3 - 22 hosts**
- **Network 4 - 23 hosts**

In order to make **4 subnetworks** you'll catch **2** bits from the host bits, so **2^2>4**. That would result in **26 network bits** in total and **6 host bits**.

But here comes the problem, the available hosts in every subnetwork it is **2^6 = 64-2 = 62 hosts**, this is less that what is required in network 2, **80 > 62**.

So to solve this problem is where it takes a different approach, Variable-Length Subnet Masking.

### Variable-Length Subnet Masking

The solution to the before problem is the VLSM, this one consist creating the subnets based on the required hosts.

Aplying this solution to the vefore problem would be as follows.

Starting network is **199.199.199.0/24**. 
You requires **4 subnets** with the given specifications:
- **Network 1 - 5 hosts** -> 2^3= 8 > 5 -> 3 host bits
- **Network 2 - 80 hosts** -> 2^7= 128 > 80 -> 7 host bits
- **Network 3 - 22 hosts** -> 2^5= 32 > 22 -> 5 host bits
- **Network 4 - 12 hosts** -> 2^4= 16 > 12 -> 4 host bits

The subnetwork will be done from major host requirements to lower host requirements, so:
- **Network 2:** 32 - 7 = 25 Network bits
    - **199.199.199.0/25** (Network Address)
    - **199.199.199.127/25** (Broadcast Address)
- **Network 3:** 32 - 5 = 27 Network bits
    - **199.199.199.128/27** (Network Address)
    - **199.199.19.159/27** (Broadcast Address)
- **Network 4:** 32 - 4 = 28 Network bits
    - **199.199.199.160/28** (Network Address)
    - **199.199.199.175/28** (Broadcast Address)
- **Network 1:** 32 - 3 = 29 Network bits
    - **199.199.199.176/29** (Network Address)
    - **199.199.199.183/29** (Broadcast Address)
- **Empty space:**
    - **199.199.199.184**
    - **199.199.199.255**

# CIDR (Classless Inter-Domain Routing)

It don't take in count the class A, B, C, D of the classful sistem it only counts the mask you put int he IP.

# IPv4 Summarization
Summarization is the process of combining multiple subnetworks into a single network usually called aggregation, this results efficient in large networks because it provides hierarchy. Some routers perform summarization by default.

Example: This 3 IPs will be summarized.
- 20.20.40.90/30 -> **20.20.40.0101**1010 |
- 20.20.40.82/30 -> **20.20.40.0101**0010 | -> 20.20.40.01010000 = **20.20.40.80/28 summarized IP**
- 20.20.40.88/29 -> **20.20.40.0101**1000 |

# IPv4 Supernetting
It consists in aggregating multiple networks into a single network, that breaks classfull boundaries. Supernetting can only be done manually.
Example: This 2 IPs will be supernetted:
- 192.168.1.0/24 -> **192.168.000000**01.00000000 |
- 192.168.2.0/24 -> **192.168.000000**10.00000000 |-> 192.168.000000hh.hhhhhhhh = **192.168.0.0/22 supernetted IP**
- 192.168.3.0/24 -> **192.168.000000**11.00000000 |
