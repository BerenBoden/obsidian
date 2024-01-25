### *Spanning tree algorithm*
The Spanning Tree Algorithm (STA) is the computational logic behind the Spanning Tree Protocol (STP). Its primary function is to prevent network loops in Ethernet LANs while still maintaining network redundancy for failover paths. Here's how it generally works:
- Root Bridge Selection: All switches in the network participate in an election process to choose a root bridge. The switch with the lowest bridge ID (a combination of priority value and MAC address) becomes the root.
- Role Assignments: After the root bridge is elected, every switch port is assigned a role based on its relationship to the root bridge. The roles can be root port (RP), designated port (DP), or blocked port.
- Path Cost Calculation: STA calculates the cost of the path from each switch to the root bridge. The path cost is determined based on the speed of the links.
- Best Path and Redundant Link Identification: Each switch identifies its best path to the root bridge and marks this as its root port. It also identifies the designated port for each network segment. All other ports are put into a blocked state, effectively disabling redundant links.
- Loop Prevention: By blocking certain ports, STA ensures that there is only one active path between any two network devices, thus preventing loops.
- Topology Changes: If a link fails or a new link is added, STA recalculates the tree and updates the port roles as necessary to adapt to the new topology.

By doing all this, STA helps in creating a loop-free, logical tree structure that spans all switches in a network, ensuring efficient data packet delivery.
### *Bridge Protocol Data Units*
BPDUs are a crucial part of how the Spanning Tree Algorithm (STA) functions. They are the mechanism by which switches exchange information needed for the STA to perform its tasks like root bridge selection, role assignments, and topology changes. BPDUs are sent out periodically by all switches to discover the network topology and to share this information with other switches. This data is then used by the STA to make the necessary calculations and decisions for maintaining a loop-free network. 
### *Types of STP*
There are 4 different types of STP:
- **Original STP** 
  - STP / 802.1D 
- **PVST+** 
  - Cisco improvement adding a per VLAN feature 
  - Cisco default 
- **RSTP / 802.1w** 
  - Improved STP with much faster convergence 
- **Rapid PVST+** 
  - Cisco improvement of RSTP adding per VLAN feature 
  - Makes a large network more efficient
### *STP port roles*
- **Root ports**. The best port to reach the Root Bridge
- **Designated port**. Port with the best route to the Root Bridge on a link
- **Non-designated port**. All other ports that are in a blocking state
### *STP states*
There are five different states of STP:
- **Blocking**. A blocked port doesn't forward frames, it only listens to BPDUs for network topology changes and drops all other frames. This prevents looped paths. 
- **Listening**. The port listens to BPDUs to ensure no network loops are occurring before passing frames.  The port will forward frames, but not update the MAC database.
- **Learning**. The port listens to BPDUs for network topology changes, but does not forward any frames. The switch will update the MAC database. Forward delay is the time it takes to transition a port from listening to learning mode. It’s set to 15 seconds by default.
- **Forwarding**. The port sends and receives all frames. If the port is still a designated or root port at the end of the learning state it enters the forwarding state.
- **Disabled**. A port in the disabled state is set administratively and is virtually non-operational 
### *Convergence in STP*
Convergence is the time it takes for the network to become stable with STP. Below is a simple packet tracer setup that uses a network of switches to efficiently reduce the amount of time it takes to converge.
![image](https://beren-obsidian-images.imgix.net/feb42014358169570ba237e7661cd0ee.png)

By implementing a physical design with a dedicated root switch, you can lower the amount of time it takes to converge your network.
### *Rapid Spanning Tree Protocol*
Rapid Spanning Tree Protocol (RSTP) is an enhancement or evolution of the original 802.1D STP. The main advantage RSTP offers is significantly faster convergence times for your network. While traditional STP can take up to 50 seconds to converge, RSTP aims to bring that time down to about 5 seconds or even less. This quick convergence is especially useful in dynamic network environments where high availability and minimal downtime are critical.

Another important point is that RSTP is backward compatible with the original 802.1D STP, making it easier for network administrators to implement RSTP without having to make radical changes to their existing network setups.

The protocol also simplifies port states for easier understanding and management. In RSTP, most states are categorized under 'discarding', 'learning', and 'forwarding', making it more streamlined compared to the various port states in 802.1D STP.

In Packet tracer you can enter the command `sh spanning-tree` to verify if you are running the traditional 802.1D STP or the faster RSTP. This result shows me I am running 802.1D because the output shows `Spanning tree enabled protocol ieee`.
```
Switch>sh spanning-tree
VLAN0001
Spanning tree enabled protocol ieee
Root ID Priority 32769
Address 0001.648C.EE28
This bridge is the root
Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
Bridge ID Priority 32769 (priority 32768 sys-id-ext 1)
Address 0001.648C.EE28
Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
Aging Time 20
Interface Role Sts Cost Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa4/1 Desg FWD 19 128.5 P2p
```

If the switch is running RSTP, you should see something like `Spanning tree enabled protocol rstp` or simply `RSTP` somewhere in the output.
### *Resources*
https://www.youtube.com/watch?v=japdEY1UKe4
https://en.wikipedia.org/wiki/Spanning_Tree_Protocol
Chapter 11 - Page 435 Todd Lammle Network+ Study Guide