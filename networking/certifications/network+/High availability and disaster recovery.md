### *Hardware redundancy and fault tolerance*
#### **Multipathing**
Multipathing is when a host is using multiple Host Bus Adapters (HBAs) to connect to multiple switches, and those links from the switches are connected to multiple storage processors with each is connected to an array of storage devices. 

Multipathing provides high fault tolerance against HBA failures, path failures, switch failures and storage processor failures.
#### **NIC teaming**
Adding two NICs to a device is called NIC teaming. NIC teaming is used for bandwidth aggregation and failover in case a NIC malfunctions. There are two modes, specifically; active/active which is used for load balancing and active/passive which is used for redundancy in case the active NIC fails.

Static teaming is when both of the NICs on the device are connected to one switch. Switch independent setup is when both of the NICs are connected to separate switches.
#### **Switch stacking**
Switch stacking is when you have multiple switches on the same rack that are connected together so you can manage them all as one switch. A switch stack always has one active switch and a standby switch in case the active switch fails. The active switch is used to manage all the other switches in the stack. 

There are drawbacks to a traditional switch stack topology:
■ Overhead of management.
■ STP will block half of the uplinks.
■ No direct communication between switches

The Cisco software StackWise solves those drawbacks, StackWise is used to connect switches in the same stack together. This saves the upfront cost of purchasing one large enterprise switch, instead you can purchase smaller switches and add them one by one, then use StackWise to connect the new switches.

To create a StackWise unit, you combine switches into a single, logical unit using special stack interconnect cables. When you add a new switch to the StackWise stack, it automatically configures the unit with the currently running IOS image as well as the configuration of the stack. Here are some more features of StackWise:
- **Dynamic Updates**: StackWise continuously updates network topology and routing information across all switches in the stack.
- **Unified Management**: A master switch, chosen from the stack members, oversees the entire stack as one unit.
- **Stack Capacity**: Up to nine switches can be connected in a single stack.
- **Single IP Management**: The entire stack is managed using one IP address, with a unified configuration file distributed across all switches.
- **Management Efficiency**: While StackWise introduces some management tasks, it allows for the creation of EtherChannel links, reducing the need for STP.
- **Logical Unit Formation**: StackWise combines multiple switches into one logical entity.
- **Interconnect Cables**: Special cables are used to connect and unify the switches.
- **Simplified Administration**: The stack functions as a single entity, simplifying management and reducing administrative overhead.
- **EtherChannel Over STP**: Using EtherChannel with StackWise eliminates the need for STP.
- **Stack Size**: The technology supports stacking up to nine switches together.
#### **Switch clustering**
You can use the Cluster Management Protocol (CMP), which operates at the data link layer, to create a cluster of switches that is managed by a single cluster commander switch, the other switches are called cluster members. This way you can configure and troubleshoot the cluster members through the single IP address of the cluster commander switch. 
#### **First Hop Redundancy Protocols**
**Routers**
FHRPs allow you to configure more than one physical router to appear as if they were only a single logical one. FHRPs like HSRP (Hot Standby Router Protocol), VRRP (Virtual Router Redundancy Protocol), or GLBP (Gateway Load Balancing Protocol) are commonly used for this purpose. 

The steps to setup a virtual router in a FHRP network involves this configuration:
1. **Define the Virtual Router**:
- The virtual router is defined by a virtual IP address and, in some protocols, a virtual MAC address. This virtual IP is what the devices on the LAN will use as their default gateway.
2. **Select the FHRP Protocol**:
- Choose the appropriate FHRP based on your requirements. HSRP and VRRP are quite similar, providing redundancy but typically only allowing one active router at a time. GLBP allows load balancing across multiple routers.
3. **Configure Participating Routers**:
- Assign two or more physical routers to participate in the FHRP group. These routers will share the virtual IP address.
- Configure each router with the FHRP settings, including the virtual IP address and, depending on the protocol, a group number or name.
- Determine the roles (active, standby, or in GLBP, potentially active-active) that each router will play within the FHRP. The active router will handle the traffic for the virtual IP address, while the standby router(s) will take over if the active router fails.
4. **Set Priorities**:
- Assign priorities to the routers to determine which one will be the active router and which will be standby. The router with the highest priority will usually become the active router.
5. **Preempt Settings**:
- Configure preempt settings if you want a router with a higher priority to take over as the active router automatically when it comes online after a failure or initial startup.
6. **Timers**:
- Configure timers that control how often the routers send hello messages to each other and how long a router will wait without receiving a hello message before declaring the active router as down.
7. **Additional Features** (Depending on the Protocol):
- For HSRP, you might configure a virtual MAC address.
- For VRRP, a virtual router master is automatically assigned a virtual MAC address.
- For GLBP, you can configure multiple virtual MAC addresses for load balancing purposes.
8. **Test the Configuration**:
- Verify the FHRP configuration and test failover to ensure that the standby router takes over seamlessly if the active router fails.

**Firewalls**
Firewalls can also be clustered and use FHRPs. You can cluster firewalls to act as a single logical one so the firewall cluster looks like a single logical device (a single MAC/IP address) to the network, this is to load balance and provide redundancy.

**HSRP**
Hot Standby Router Protocol (HSRP) is a Cisco proprietary redundancy protocol for establishing a fault-tolerant default gateway. Routers are configured to be part of an HSRP group. Each group represents a single virtual router with a virtual IP and MAC address. Within the group, one router is elected as the Active router, and another as the Standby router. Other routers (if any) remain in a listen state. 

HSRP-specific messages are sent over multicast to the IP address 224.0.0.2 (for IPv4) or FF02::66 (for IPv6), on UDP port 1985. These packets contain information essential for the HSRP election process and maintaining the current HSRP state. They include the priority of the sending router, the current state of the router, the virtual IP address, and the group number. The active and standby routers are determined based on priorities set in the HSRP configuration (higher priority wins). If priorities are equal, the highest IP address becomes the deciding factor.

The Active router assumes responsibility for routing traffic sent to the virtual router's IP address. It responds to ARP requests with the virtual MAC address. The Standby router monitors Hello messages from the Active router and is ready to assume the role of the Active router if the current Active router fails. If the Standby router stops receiving Hello messages from the Active router within a set period (Hold time), it assumes the Active router role. This involves taking over the virtual IP address and responding to ARP requests.

### *Questions*
Technique used to spread work out to multiple computers,
network links, or other devices; Load balancing ✅

Allows multiple network interfaces to be placed into a team
for the purposes of bandwidth aggregation; NIC teaming ✅

Devices that can immediately supply power from a battery
backup when a loss of power is detected; UPS ✅

A leased facility that contains all the resources needed
for full operation; Cloud sites X FUCKING HOT SITE

A Cisco proprietary FHRP; HSRP ✅
4/5

1. Which of the following backup types does not include the data? ✅
- [ ] A. Full
- [x] B. System state
- [ ] C.Clone
- [ ] D.Differential
2. Which of the following is a measure back in time to when your data was preserved in a
usable format, usually to the most recent backup?  ✅
- [ ] A. RTO
- [ ] B. MTBF
- [x] C.RPO
- [ ] D.MTD
3. Which of the following is an IEEE standard (RFC 2338) for router redundancy? ✅
- [ ] A. HSRP
- [x] B. VRRP
- [ ] C.HDLC
- [ ] D.MLPS
4. Which of the following is the defined interval during which each of the routers send out
Hello messages in HSRP? X HELLO TIMER
- [x] A. Hold timer
- [ ] B. Hello timer
- [ ] C.Active timer
- [ ] D.Standby timer
5. What is the HSRP group number of the group with the following HSRP MAC address?
0000.0c07.ac0a ✅
- [x] A. 10
- [ ] B. 15
- [ ] C.20
- [ ] D.25
6. Which of the following only provides fault tolerance? ✅
- [ ] A. Two servers in an active/active configuration
- [ ] B. Three servers in an active/passive configuration with one on standby
- [x] C.Three servers in an active/passive configuration with two on standby
- [ ] D.Three servers in an active/active configuration
7. Which site type mimics your on-premises network yet is totally virtual? ✅
- [ ] A. Cold site
- [x] B. Cloud site
- [ ] C.Warm site
- [ ] D.Hot site
8. Which of the following fire suppression systems is not a good choice where computing equipment will be located? ✅
- [x] A. Deluge
- [ ] B. Wet pipe
- [ ] C.Dry pipe
- [ ] D.Preaction
9. Which of the following protocols gives you a way to configure more than one physical router
to appear as if they were only a single logical one? ✅
- [x] A. FHRP
- [ ] B. NAT
- [ ] C.NAC
- [ ] D.CMS
10. Which of the following provides a method to join multiple physical switches into a single
logical switching unit? ✅
- [x] A. Stacking
- [ ] B. Daisy chaining
- [ ] C.Segmenting
- [ ] D.Federating

9/10
### *Resources*
- https://www.dell.com/support/kbdoc/en-nz/000023822/dell-emc-unity-understanding-storage-processor-high-availability-user-correctable
- [Stacking cables a better way](https://community.cisco.com/t5/networking-blogs/connecting-stack-cables-a-better-way/ba-p/4109155)
- [CMP vulnerability](https://www.cisco.com/c/en/us/support/docs/csa/cisco-sa-20190327-cmp-dos.html)