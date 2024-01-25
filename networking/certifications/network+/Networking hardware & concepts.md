![image](https://beren-obsidian-images.imgix.net/5b5e656af6e522e8b596aa0c2170fd52.jpg)

### Connection Devices
The following table lists several common connection devices used within a LAN.

|Device|Description|
|---|---|
|Hub|A _hub_ is the central connecting point of a physical star, logical bus topology. Hubs manage communication among hosts using the following method:<br><br>1. A host sends a frame to another host through the hub.<br>2. The hub duplicates the frame and sends it to every host connected to the hub.<br>3. The host to which the frame is addressed accepts the frame. Every other host ignores the frame.<br><br>Hubs are Layer 1 devices; they simply repeat incoming frames without examining the MAC address in the frame.|
|Bridge|A _bridge_ is a device that connects two (or more) media segments on the same subnet, and it filters traffic between both segments based on the MAC address in the frame. A bridge builds a database based on MAC addresses to use for making forwarding decisions.<br><br>- The process begins by examining the source MAC address of an incoming frame. If the source address is not in the forwarding database, an entry for the address is made in the database, associating the MAC address with the media segment.<br>- The destination address is then examined.<br>    - If the destination address is not in the database, the frame is sent out on all segments except for the one on which it was received.<br>    - If the destination address is in the database, the frame is forwarded to the appropriate segment if the segment is different from the one on which it was received.<br>    - Broadcast frames are forwarded to all segments except the one on which they were received.<br><br>You should be aware of the following regarding bridges:<br><br>- Bridges are used to separate one part of a subnet from another. This eliminates unnecessary traffic between segments and keeps the network from wasting bandwidth.<br>- All segments connected to a bridge are on the same subnet and share a common subnet address.<br>- Bridges can connect two segments that use different types of network architecture. For example, a bridge can connect a segment using Ethernet to a segment using 802.11 wireless.<br>- Bridges are Layer 2 devices; they read the MAC address contained in a frame to make forwarding decisions.<br>- Frame forwarding happens independently of the upper-layer protocols (such as TCP/IP).|
|Switch|A _switch_ is a multi-port bridge that performs filtering based on MAC addresses and provides additional features not found in a bridge.<br><br>- While most bridges can process only a single frame at a time, switches can process multiple frames simultaneously.<br>- Switches offer guaranteed bandwidth to each switch port.<br>- Switches can make additional forwarding decisions based on the MAC address. For example, a switch can be configured to accept frames from specific MAC addresses.<br>- Like bridges, switches operate at Layer 2.<br>- Unmanaged switches are autonomous in their function, requiring no port management or configuration. Managed switches allow administrators to change the port configurations, including the following:<br>    - Port speed<br>    - Duplexing<br>    - Filters based on network adapter MAC addresses<br>    - VLAN assignment|
|Wireless Access Point (WAP)|A _wireless access point_ (WAP) is a hub for a wireless network.<br><br>- As with a hub, any message sent to any wireless host connected to the AP can be received by all other wireless hosts.<br>- An AP is a Layer 2 device; it can read the Data Link layer address in a frame.<br>- An AP is often configured as a bridge, connecting a wireless segment to a wired segment. Both wireless and wired hosts are on the same subnet.<br>- Some APs are combination devices that include a wired switch and even a router.|
|Repeater|A repeater receives, regenerates, amplifies, and retransmits the electrical signals it receives. It also removes the effects of attenuation. A hub can sometimes be called a repeater because its main function is to accept an electrical signal coming in on one port and forward that same signal out to all of the other ports. However, many other devices can also be used as a repeater.|
Devices such as hubs, switches, and bridges connect multiple devices to the same network segment. Internetwork devices connect multiple networks or subnets together and enable communication between hosts on different types of networks.
### Common Internetworking Devices

The following table lists several common internetworking devices.

|Device|Description|
|---|---|
|Router|A _router_ is a device that connects two or more network segments or subnets.<br><br>- Each subnet has a unique logical network address.<br>- Routers can be used to connect subnets within a single LAN, or they can be used as gateways to connect multiple LANs together.<br>- Routers can be used to connect networks with different architectures (for example, connecting an Ethernet network to a token ring network).<br><br>Routers maintain information about other networks in a database called a _routing table_ . The routing table typically contains the address of all known networks and the next router in the path used to reach the destination network. The routing table is used in the process of forwarding packets.|
|Firewall|A _firewall_ is a router with additional security features. Firewalls can be programmed with security rules to restrict the flow of traffic between networks.<br><br>- Firewall rules control the type of traffic allowed into a network and the type of traffic allowed out of a network.<br>- A firewall can be either hardware devices or software installed onto operating systems.|
|Layer 3 switch|A Layer 3 switch is capable of reading Layer 3 (network) addresses and routing packets between subnets. A Layer 3 switch often provides better performance than a router, but it does not support as many features as a router.|

Each of the devices listed in this table operates at the Network layer (Layer 3) of the OSI model. Some firewalls are also capable of operating at higher layers and make filtering decisions based on information found in the upper OSI model layers.

### Messaging Process

Routers receive packets, read their headers to find addressing information, and send them on to their correct destination on the network or internet. The following process is used to send a message from one host to another on a different network:

1. The sending host prepares a packet to be sent. The host uses its own IP address as the source Network layer address and the IP address of the final receiving device as the destination Network layer address.
2. The sending host creates a frame by adding its own MAC address as the source Physical layer address. For the destination Physical layer address, the host uses the MAC address of the default gateway router.
3. The sending host transmits the frame.
4. The next hop router reads the destination MAC address in the frame. Because the frame is addressed to that router, it processes the frame.
5. The router strips off the frame header and examines the packet destination address. It uses the routing table to identify the next hop router in the path.
6. The router repackages the packet into a new frame. It uses its own MAC address as the source Physical layer address and the MAC address of the next hop router as the destination Physical layer address.
7. The router transmits the frame.
8. The next hop router repeats steps 4 - 7 as necessary until the frame arrives at the last router in the path.
9. The last router in the path receives the frame and checks the destination IP address contained in the packet.
10. Because the destination device is on a directly connected network, the router creates a frame using its own MAC address as the source Physical layer address and the MAC address of the destination device as the destination Physical layer address.
11. The router transmits the frame.
12. The destination device receives the frame. Inside the packet, it finds that the destination Network layer address matches its own IP address, and the source IP address is that of the original sending device.

Note the following:

- Both Data Link layer physical addresses and Network layer logical addresses are used to send packets between hosts on different subnets.
- IP (Network layer) addresses are contained in the IP header; MAC (Data Link layer) addresses are contained in the Ethernet frame header.
- A router uses the logical network address specified at the Network layer to forward messages to the appropriate network segment.
- Data Link addresses in the frame change as the frame is delivered from hop to hop. At any point in the process, the Data Link destination address indicates the physical address of the next hop on the route. The Data Link source address is the physical address of the device sending the frame.
- Network addresses remain constant as the packet is delivered from hop to hop. The Network addresses indicate the logical address of the original sending device and the address of the final destination device.
### Equipment Organization
Data centers can quickly become crowded and disorganized as equipment is installed, removed, and relocated due to organizational changes. If you don't take steps to keep things organized, you'll likely end up in a situation where you have various servers and cabling deployed haphazardly throughout the center.

The following table describes a few methods to keep your data center organized.

|Tool|Description|
|---|---|
|Racks|A rack can house multiple network devices in a vertical stack. Racks also make it much easier to organize network cabling. In addition, the rack allows the following devices to share physical resources such as:<br><br>- Cooling fans<br>- Power supplies<br>- Peripherals using a keyboard, video, and mouse (KVM) device<br><br>![Computer rack](https://cdn.testout.com/_version_6026/netpro2021v6-en-us/en-us/resources/text/t_data_center_install_np6/rack.jpg)|
|Blade devices|Blade devices are very thin. They are designed to provide high device density and the most efficient use of available rack space.|
### Labeling

A hardware/wiring configuration that makes perfect sense when you initially put it together may be mystery a year later when you try to troubleshoot problems with it. Consider labeling:

- Switch ports
- Patch panel connections
- Wall jacks
- Electrical circuits
- Devices and systems

When implementing a labeling plan in a data center, you should define (and abide by) a naming convention to uniquely identify each component. The naming convention you use will vary based upon the network and the organization's needs. It is important that you are consistent with the naming convention you decide to use.

Consider the following:

- All file servers are named FSx (such as FS1, FS2, and so on)
- All printers are named PTRx
- All workstations are named WSx
- All switches are named SWx
- All routers are named RTRx

### Physical Network Diagrams

Network diagrams are extremely useful. They help you manage and maintain the network infrastructure regarding security, troubleshooting, downtime, compliance, etc. They are like maps that depict objects and connections in the network using shapes, icons, symbols, and other visuals.

There are different types network diagrams that fulfill different purposes. The following table describes a few of the most commonly used diagrams:

|Network Diagram|Description|
|---|---|
|Floor plan|A floor plan diagram shows the physical location of each piece of network equipment within the company's premises. Making a network floor plan is often done using software that creates floor plan diagrams.  <br>  <br>The software creates a visual representation of physical components along with a clearly defined set of principles and procedures.|
|Rack diagram|A rack diagram, or rack elevation, is a map of the layout of IT equipment within a server rack. It's used to manage and track data center assets. Just as with the floor plan diagram, there are software options available to help you create a rack diagram more easily.|
|Main distribution frame (MDF)/intermediate distribution frame (IDF) documentation|The MDF is a cable rack that manages and connects the cables entering a building.  <br>  <br>The IDF is a cable rack that manages and connects wiring for communication between the MDF and workstation devices. The cables run through the MDF, are distributed to each IDF, and finally go to specific workstations.  <br>  <br>Documentation relating to IDF and MDF includes the following:<br><br>- Standard Operating Procedure (SOP) - This document is a set of step-by-step instructions to help IT maintenance workers with complex routine operations. The instructions are very detailed to minimize the chance for errors.<br>- Policies can be created as a guide and documentation for IDF and MDF. Policies are usually placed in the organization's quality manual. Policies give a course of action to guide and influence decisions.|
|Logical network diagram|A logical network diagram shows how traffic flows across the network. It includes information such as IP addresses, admin domains, how domains are routed, control points, and so on.  <br>  <br>If you're using the OSI model, logical diagrams are referred to there as L2.|
|Wiring diagram|A wiring diagram is a map of the physical connections and physical layout of the electrical system/circuit in a building. It shows how the electrical wires are connected. It can also display where fixtures and components can connect to the system.|