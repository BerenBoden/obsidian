### *Address learning in switches*
Switches are layer 2 devices that filter and forward frames to devices based on their MAC addresses, these MAC addresses are saved to a MAC database or MAC forward/filter table on the switch so that the switch can remember where to send frames to each device connected to the switch. In this scenario I have created a simple LAN with one switch, and 4 PCs.
![image](https://beren-obsidian-images.imgix.net/814acc0f200dd8f5483a8f38b8eb069b.png)

To display the MAC database on a switch you can run `show mac address-table`, initially it is empty because no frames have been routed and I have not yet configured the PCs to have IP addresses.
```
Switch>show mac address-table
Mac Address Table
-------------------------------------------
Vlan Mac Address Type Ports
---- ----------- -------- -----
```

After configuring the PCs to have IP addresses, and setting the default gateway, in many cases one of the following may occur:
- ARP (Address Resolution Protocol) requests may be sent to resolve IP addresses to MAC addresses.
- DHCP (Dynamic Host Configuration Protocol) frames may be sent if the device is configured to obtain an IP address automatically.
- Some operating systems may send out network discovery packets to identify other devices on the network.

If none of the above happens, then lets say the first packet being sent is an ICMP echo request from PC 8 to PC 11 (192.168.1.2> ping 192.168.1.4). PC 8's MAC address is taken from the frame and saved to the MAC database along with the source port, however, the switch does not know which device to send the frame to, so it will flood the frame through every port (except the source port), until the device the ping request was destined for responds with a frame, it will then take the source MAC address of that frame and then it will save it to the MAC database along with the port that the device is connected to. 

the MAC database now looks like this. All PCs are connected to VLAN 1 and their MAC addresses have been saved to the MAC database. This is after each device has sent a frame to the switch.
```
Switch>show mac address-table
Mac Address Table
-------------------------------------------
Vlan Mac Address Type Ports
---- ----------- -------- -----
1 0001.6321.6a52 DYNAMIC Fa2/1
1 0004.9ac2.3854 DYNAMIC Fa0/1
1 0007.ec00.684a DYNAMIC Fa3/1
1 0007.ec4a.3206 DYNAMIC Fa1/1
```

The MAC database is how the switch remembers the device. Now the switch can make more efficient forwarding decisions. Instead of flooding the frame out of all ports (except the source port), the switch can now forward the frame directly from PC 11 device to PC 8, effectively making a point-to-point connection between them through the switch.
### *Loop avoidance*
A redundant link between two switches are helpful as backups for each other. If one link fails, the other can take over, maintaining the network's availability. However, this may introduce network loops. If no loop avoidance mechanisms are introduced, this will create a broadcast storm, continuously flooding broadcast frames through the network. These are a few issues that can be caused by not implementing loop avoidance in a network with redundant failover links:
- Broadcast Storms: Endless flooding of broadcast frames throughout the network, which can severely degrade network performance.
- Multiple Frame Copies: A device can receive multiple copies of the same frame from different segments, causing additional network overhead.
- MAC Table Confusion: The MAC address filter table can become confused about device locations, leading to inefficient frame forwarding. This is referred to as "thrashing the MAC table."
- Nested Loops: The presence of multiple loops within the network can exacerbate the above issues, making the network essentially inoperable if a broadcast storm occurs.

All of these problems can be avoided by implementing STP ([[Spanning Tree Protocol (STP)]]) which is a loop avoidance mechanism.
### *Spanning tree protocol*
Most switches by default run the 802.1D IEEE version of STP, which was one of the earlier versions of STP created by IEEE. However, the 802.1D version of STP is slower than the more modern 802.1q version of STP, which, by default, is not enabled on any switch. More types of STP are discussed here [[Spanning Tree Protocol (STP)#*Types of STP*]]

STP was created to stop network loops from occurring at layer 2. STP uses STA ([[Spanning Tree Protocol (STP)#*Spanning tree algorithm*]]) STA will create a topology of the network and find and remove any redundant links. When STP is running, frames will be forwarded out only on STP-picked links. BPDUs ([[Spanning Tree Protocol (STP)#*Bridge Protocol Data Units*]]) are sent out by switches that are using STP to share information about the network topology.

I have created a breakdown of the steps that STP takes from start to finish:
- **Step 1: Initialize in Blocking State**
    - When a switch powers up, all ports start in a blocking state by default.
- **Step 2: Evaluate Cost to Root Bridge**
    - Each port evaluates its path cost to the root bridge.
- **Step 3: Transition to Forwarding State**
    - The port with the lowest cost transitions to a forwarding state.
- **Step 4: React to Topology Change**
    - If there's a change in network topology due to a failed link or a new switch, ports may transition to listening and learning states.
- **Step 5: Re-evaluate Best Path**
    - In the case of a topology change, the switch reassesses the best path to the root bridge.
- **Step 6: Blocking Ports**
    - Ports that are not on the best path go back to a blocking state to prevent network loops.
- **Step 7: Receive BPDUs in Blocking**
    - Even in a blocking state, ports can still receive BPDUs but don't send out any frames.
- **Step 8: Potential Role Change**
    - If a blocked port is now better suited to be the designated or root port, it transitions to listening state.
- **Step 9: Listen for BPDUs**
    - In listening state, the port checks all received BPDUs to make sure it won’t create a loop.
- **Step 10: Transition to Forwarding**
    - Once it's determined that it won't create a loop, the port goes back into a forwarding state.
### *VLANs*
By default, switches break up collision domains and routers break up broadcast domains. So how do you break up broadcast domains in a switch? Since a switch is one broadcast domain, you can use VLANs to virtually segment your switch into different broadcast domains.

A VLAN is a logical grouping of administered ports on a switch. Each VLAN can not communicate with other hosts on a different VLAN, so if you need inter-VLAN communication, you will still need a router.
### *Port security*
Port security is a way of securing your switches so that no unauthorized users can plug a device into a switch and connect to the network. Port security is based on MAC addresses, by using port security, you can limit the number of MAC addresses that can be assigned dynamically to a port, set static MAC addresses, and shut down a port when a security policy is violated.
### *DHCP snooping*
DHCP snooping is a security feature implemented on switches to protect against rogue DHCP servers that could potentially distribute incorrect IP addresses or act maliciously within a network.

The primary goal of DHCP snooping is to only allow DHCP responses from trusted DHCP servers, thereby stopping rogue DHCP servers from assigning IP addresses. This mitigates security risks such as unauthorized access or denial of service attacks.

You define which switch ports are "trusted" and which are "untrusted." Typically, the port connecting to your legitimate DHCP server would be marked as trusted. When a client on an untrusted port sends out a DHCP request, the switch allows it to pass through to the trusted port where the actual DHCP server is. Replies from the DHCP server are allowed back through the trusted port and on to the client. Replies from DHCP servers on untrusted ports are blocked.

**Bindings table**
The switch also maintains a bindings table that maps IP addresses to MAC addresses. This table is populated by snooping on the DHCP traffic between clients and the legitimate DHCP server. Any incoming packets with IP-to-MAC mappings that do not match the bindings table will be dropped, providing an additional layer of security.

**ARP inspection**
ARP Inspection is a security feature on switches that helps to prevent man-in-the-middle attacks by making sure that the data packets are really coming from where they claim to be coming from. It does this by checking the MAC addresses in the packets against a trusted list of MAC-to-IP address pairs. This trusted list is built using DHCP snooping, another security feature.

Here is an example MITM attack that could be prevented with ARP inspection
- **Normal Scenario**: Jim wants to talk to Bob. He asks, "Who has Bob's IP?" and Bob's computer says, "It's me, and here's my MAC address." Jim saves this in his ARP cache and talks directly to Bob.
    
- **Attack Scenario Without ARP Inspection**: Leroy wants to intercept Jim's messages to Bob. When Jim asks, "Who has Bob's IP?" Leroy jumps in and says, "It's me, and here's my MAC address." Now, Jim thinks he's talking to Bob but is really talking to Leroy, who can intercept and even modify messages before sending them on to Bob.
    
- **With ARP Inspection**: The switch knows that Bob's IP should match a specific MAC address because of DHCP snooping. When Leroy tries to impersonate Bob, the switch checks the MAC address. It sees that Leroy's MAC doesn't match Bob's IP and blocks the traffic, stopping Leroy's attack.
### *Flood guard*
An attacker may be able to flood a switch with random MAC addresses, causing the switch to flood the MAC database with the random MAC addresses. This would lead to the MAC database overflowing with random MAC addresses, now any legitimate frames arriving to the switch will have an unknown MAC address as it will not be in the MAC database, as the attacker has flooded it, the attacker can now sniff the traffic because the switch will flood the legitimate frame out all ports as it is not in the MAC database. 

A flood guard can prevent this attack, it uses two mechanisms to accomplish this; Unknown Unicast Flood Blocking (UUFB) and Unknown Unicast Flood Rate-limiting (UUFRL). The UUFB feature blocks unknown unicast and multicast traffic flooding at a specific port, only permitting egress traffic with MAC addresses that are known to exist on the port. The UUFRL feature applies a rate limit globally to unknown unicast traffic on all VLANs
### *BPDU guard*
BPDU Guard is a feature designed to protect your network from unauthorized devices pretending to be switches. Normally, you only want to receive BPDUs on ports that are supposed to be connected to other switches or network backbone devices, not from end-user devices.

BPDU Guard helps to make sure that only authorized switches in your network can participate in STP. It's generally enabled on ports that connect to end devices (like PCs, printers, etc.) and not on ports that connect to known switches or routers.

If a port with BPDU Guard enabled receives a BPDU packet, the feature shuts down the port. This stops any device that is not an authorized switch from sending BPDUs and potentially affecting the network topology.
### *Root guard*
Root Guard is a feature that helps maintain a stable STP network topology. It's similar to BPDU Guard, but with a different focus.
- Applied to interfaces on the current root bridge.
- Stops those ports from becoming "root ports," which would indicate a different path to a new root bridge.
- If a BPDU with a superior priority is received, the feature disables the port to prevent a new root bridge election.

In short, Root Guard ensures that the current root bridge remains the root bridge by preventing other switches from taking over that role.

If an attacker successfully installs a rogue switch and manages to make it the root bridge in a Spanning Tree Protocol (STP) environment, several negative outcomes could occur:
- Traffic Redirection: The rogue root bridge could redirect network traffic through itself, enabling the attacker to eavesdrop on the data flowing through the network.
- Denial of Service (DoS): By manipulating STP timers or topology, the attacker could create network loops, leading to broadcast storms that make the network unusable.
- Man-in-the-Middle Attacks: The attacker could intercept and possibly alter data packets between two communicating hosts.
- Resource Exhaustion: The rogue switch could send numerous BPDUs to exhaust the CPU and memory resources on other network switches, impacting their performance.
- VLAN Hopping: If the rogue switch is also capable of sending maliciously crafted VLAN tags, it could hop between VLANs, effectively bypassing network segmentation.
### *VTP server*
A VTP (VLAN Trunking Protocol) server, is a way to configure VLANs consistently across a switched internetwork. VTP is only available for Cisco switches, it can help with many of the following benefits:
- Consistent VLAN configuration across all switches in the network
- Accurate tracking and monitoring of VLANs
- Dynamic reporting of added VLANs to all switches in the VTP domain
- Adding VLANs using plug-and-play

When a Cisco switch that supports VTP is powered up, it defaults to VTP server mode. VTP servers rely on a domain name to share VLAN information. Here is a simple Packet Tracer configuration that is set up to use a VTP server:
![image](https://beren-obsidian-images.imgix.net/d4e828d992856a78b5a6974034cc9006.png)

I created a VTP server by entering these commands:
**Switch1 (VTP Server)**
```
Switch# enable
Switch# configure terminal
Switch(config)# vtp domain vtp-server
Switch(config)# vtp password 123456
```

**Other switches**
```
Switch# enable
Switch# configure terminal
Switch(config)# vtp mode client
Switch(config)# vtp domain vtp-server
Switch(config)# vtp password 123456
```

After setting up the root switch (Switch1) as a VTP server and configuring the other switches to be in the same VTP domain, any VLANs that were configured on the VTP server should propagate to the other switches in the VTP domain.

VTP information is sent between switches only via a trunk port. There are 3 different modes for VTP, here is a breakdown of each one:
**Server Mode**
- Default mode for all Catalyst switches.
- Required for propagating VLAN information across a VTP domain.
- Only mode that allows you to create, add, or delete VLANs.
- Any changes made to VLANs are advertised to the entire VTP domain.
- VLAN configurations are saved in [[NVRAM]].

**Client Mode**
- Receives VLAN information from VTP servers.
- Forwards updates just like a VTP server but cannot make changes to VLANs.
- Ports cannot be added to a new VLAN until the client receives the new VLAN information from the server.
- VLAN information is not saved in NVRAM. If the switch is reset, this information will be deleted.
- Tip: To convert a switch to server mode, first make it a client to receive the correct VLAN information.

**Transparent Mode**
- Does not participate in the VTP domain.
- Does not share its VLAN database but will forward VTP advertisements.
- Admins can create, modify, and delete VLANs on a transparent switch.
- Keeps its own VLAN database that is not shared with other switches.
- VLAN database is stored in NVRAM but is only locally significant.
