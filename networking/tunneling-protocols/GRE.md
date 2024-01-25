##### ***What is GRE?***
GRE (Generic Routing Encapsulation) is a tunneling protocol designed to package different kinds of network protocols within IP tunnels. This means it can carry data from various protocols like EIGRP, OSPF, RIP or IPv6 through an IP network. When it comes to its structure, a GRE tunnel includes headers for the original data (referred to as the passenger protocol), the GRE protocol itself, and the protocol used to deliver the data (usually IP).

Key Characteristics of GRE:
- **Protocol Agnostic**: GRE is versatile in that it has a protocol-type field, allowing it to tunnel nearly any Layer 3 protocol.
- **Stateless**: It doesn't maintain any state information about the data packets, meaning it doesn't keep track of the communication session.
- **No Flow Control**: It doesn't regulate data packet transmission rates or provide acknowledgment of packet receipt.
- **No Inbuilt Security**: It doesn't offer any encryption or security features by itself.
- **Additional Overhead**: Using GRE adds at least 24 bytes of extra data to each packet, which can increase network latency and reduce throughput.

An example of GRE would be if we have two remote networks, and they need to be layer 2 adjacent, so they can communicate with each other, bypassing all other routers, whether that be the WAN or another third party cloud.

To illustrate this, I created a Packet Tracer configuration. I have set two routers to be bypassed, which is the WAN, I have then configured two other routers to be the remote networks that will be communicating over a GRE tunnel with IPSec. I have chosen to use IPSec because GRE does not provide security by default, and IPSec will provide proper encryption for each packet.

**!todo: Make packet tracer scenario, Make GRE header png**
##### ***Resources***
https://www.youtube.com/watch?v=ytAqv7qHGyU
https://www.youtube.com/watch?v=ZAFl4etjXs4