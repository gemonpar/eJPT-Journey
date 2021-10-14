# Communications within Broadcast Domains 

- **Broadcast** (needs no address) -> To all
- **Unicast** (requires an address: MAC) -> To a unique
- **Multicast** (requires an address: MAC with special format) -> To a group

## Networked Software Applications fall into two categories

- Ones that assume that all that need the software are in the **same broadcast domain**. Ex: **ARP**
- Those capable of **intra or inter-broadcast domain communications**.

--------------------------------------------------------------------------

## IP Address = Broadcast Domain Address

IP is used to address all types of networks, broadcast-based, p2p, ...

IP address consists in two parts:
- Network/Broadcast Domain Address.
- Unique Host address within that broadcast domain.

So when a host from one broadcast domain send data to another host in another broadcast domain, the sender will put two directions:

- The Ethernet protocol (MAC address)
- The IP Protocol (IP address)

The MAC address is used to comunicate with L2 layer of the gateway, if coincides with the destination gateway, then the frame will be sent to L2 layer of the gateway where the IP address is interpreted and if it coincides will be sent to the destination if not all the frame will be sent to the next gateway (hop).

