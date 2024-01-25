### *Unreachable gateway attack*
The Unreachable gateway attack is essentially a man-in-the-middle (MITM) attack exploiting the ICMP redirect message to manipulate the host's routing table

I have broken down the attack into three steps, including the attacker's machine, and the victim's machine:

**Network Reconnaissance**: The attacker would use tools such as `nmap` to identify active hosts and gateways on the network.
```
nmap -sn 192.168.1.0/24
```

**Gateway Takeover**: The attacker would then need to gain control over the secondary gateway G1. This could involve various techniques, such as exploiting vulnerabilities or using default credentials.

**ARP Spoofing**: Once in control of G1, the attacker might use `arpspoof` to associate their MAC address with the IP address of the legitimate gateway G2 on the victim host's ARP table.
```
arpspoof -i eth0 -t [victim IP] [gateway G2 IP]
```

**ICMP Redirect**: Next, the attacker would send an ICMP redirect message to the source host, telling it to use G1 as the gateway for the destination host.
```
iptables -A OUTPUT -p icmp --icmp-type redirect -j ACCEPT
echo 1 > /proc/sys/net/ipv4/conf/all/accept_redirects
hping3 --icmp --icmptype redirect --icmp-gw [G1 IP] -a [G2 IP] [victim IP]
```

**Man-in-the-Middle**: With traffic now routed through G1, the attacker can use `ettercap` to perform the MITM attack, reading, modifying, and forwarding the traffi- [ ] C. 
```
ettercap -T -M arp:remote / [victim IP] // / [gateway G2 IP] //
```
### *DoS & DDoS*
A major indicator of a DDoS attack is a significant spike in network traffi- [ ] C.  This is caused by a large number of bots simultaneously sending traffic to overwhelm the network. Such spikes should always be treated with suspicion. An IDS (Intrusion Detection System) will alert for these attacks and can stop them getting larger and it may also stop them in the first place. 

Smaller organizations can utilize DDoS mitigation features integrated into their load balancers, such as the TCP SYN cookie option. This feature allows the load balancer to respond when SYN requests reach a certain threshold, by dropping requests once the SYN queue is full, thus helping to prevent potential DDoS attacks.

**Botnets**
1. A botnet master distributes malware that turns computers into bots.
2. These bots connect to a hacker-controlled server for instructions.
3. When commanded by this server, the bots launch a synchronized attack.

**Smurf**
This is a DoS attack that floods the victim with spoofed broadcast ping messages.

The attacker spoofs the target's IP and sends many pings to broadcast addresses. This causes all network devices to reply at once, overwhelming the network, especially if routers aren't set to limit this traffic. 

**Local Network Attack (192.168.1.230/24):**
- The broadcast address for the subnet 192.168.1.0/24 is 192.168.1.255.
- An attacker could send pings to 192.168.1.255, spoofing the source IP as 192.168.1.230.
- All hosts in the local subnet would receive the broadcast and respond to the spoofed IP address (192.168.1.230), causing a flood of traffic directed at the victim's machine.
**Remote Network Attack (121.75.238.190):**
- To reach a specific host behind NAT, such as 121.75.238.190, the attacker can't directly ping the broadcast address because NAT typically blocks incoming traffic not requested by internal hosts.
- For an external attacker to target the internal broadcast address, they would need to compromise an internal device first or exploit a vulnerability in the NAT/router (such as UPnP or port forwarding misconfigurations) to gain access to the internal network.
- If they manage to access the internal network, they would look for the broadcast address of the subnet to which 121.75.238.190 belongs (the specific subnet would depend on the internal network configuration).

**Syn flood**
In a SYN flood attack, the attacker overwhelms a server with SYN packets, initiating TCP connections without completing the handshake. This consumes the server's memory by leaving it waiting for ACKs that never come, preventing legitimate connections and eventually rendering the server unavailable. Modern network systems have patches to mitigate this type of attack.
![image](https://beren-obsidian-images.imgix.net/983adc45f1078a934c4e671f63355461.png)

**Stacheldraht (Barbed Wire)**
Is a mixture of different techniques used together and uses TFN (Tribal Flood Network) techniques and adds encryption.

**Reflective/Amplified Attacks**
A DNS amplification attack uses exposed and vulnerable DNS servers on the internet to conceal their malicious traffic.  An attacker might issue a command to it's botnet like:
```
hping3 --udp -c 1 -p 53 -E dns_request_file -d 56 --spoof [Victim's IP Address] [DNS Resolver IP]
```

The DNS server then initiates connections to the victim's IP address, causing a DoS.
### *VLAN hopping*
**Understanding VLAN Tags**:
In a network with VLANs, Ethernet frames are tagged with a VLAN ID, indicating which VLAN the frame belongs to. Trunk links between switches carry frames from multiple VLANs, each tagged with the appropriate VLAN ID. 

**Double Tagging Technique**:
The attacker creates an Ethernet frame with two VLAN tags - the first is the target VLAN ID, and the second is the VLAN ID of the attacker's own VLAN. The frame is sent to a switch where the attacker's device is located. 

**Tag Stripping and Misdirection**:
The first switch strips off the outer VLAN tag (the attacker's VLAN) and forwards the frame to the trunk link. On reaching the second switch, only the inner tag (the target VLAN ID) remains, which is mistakenly read by the switch, causing it to forward the frame to the target VLAN.

**Example Commands for VLAN Hopping Attack:**
Assume the attacker is on VLAN 10 and wants to access VLAN 20. The attacker's device needs to support crafting custom Ethernet frames (software like Yersinia or Scapy can be used).

**Frame Crafting** (using Scapy):
- Import necessary modules: `from scapy.all import *`
- Craft a frame: `frame = Ether()/Dot1Q(vlan=10)/Dot1Q(vlan=20)/IP(dst="target_IP")/TCP()`
- This command creates a double-tagged frame, first with the attacker's VLAN (10), then the target VLAN (20).

Send the frame: `sendp(frame)`. This sends out the crafted frame onto the network.

Modern switches can be configured to mitigate such attacks, e.g., by disabling unused ports, setting up VLAN access lists, and properly configuring trunk ports.

Read more about VLANs here: [[Switching & VLANs]]
### *ARP cache poisoning*
ARP cache poisoning or ARP spoofing is a technique used in man-in-the-middle (MITM) attacks on local area networks. Here's what it means to "map the attacker's MAC address with the victim's IP address" in this context:

**ARP (Address Resolution Protocol)**: ARP is a protocol used in local networks to find the physical MAC address that corresponds to a given IP address. Devices maintain an ARP cache, which is a table that maps IP addresses to MAC addresses.

**ARP Cache Poisoning**: In this attack, the attacker sends falsified ARP messages over a local network. This results in the ARP cache of the targeted devices (the victims) being 'poisoned'.

**Mapping the Attacker’s MAC Address with the Victim’s IP Address**: The attacker sends ARP responses to the victims, even though they didn't send an ARP request. These responses falsely tell the victims that the attacker's MAC address corresponds to the IP address of the other victim. As a result, the victims' ARP caches are updated with incorrect information: they now associate the attacker's MAC address with the IP address of the other victim.

**Interception of Traffic**: Once the ARP caches are poisoned, when one victim tries to communicate with the other, the data packets are sent to the attacker's MAC address instead of the intended recipient. The attacker can then intercept, read, or modify the data before passing it along to the actual destination, effectively positioning themselves in the middle of the conversation without either victim realizing it.
### *Questions*
1. A is a group of computers connected on the Internet for the purpose of performing a task in a coordinated manner. *Botnet*
2. How often should you update your virus definitions in your antivirus software? *regularly or automatically*
3. What type of attack injects a command that overflows the amount of memory allocated and executes commands that would not normally be allowed? *Buffer overflow*
4. attacks are those that increase the effectiveness of a DoS attack. *Amplified*
5. What kind of tool could a hacker use to intercept traffic on your network? *Ettercap / wireshark*
6. What type of virus uses Microsoft’s Visual Basic scripting language? *?*
7. What is it called when someone intercepts traffic on your network that’s intended for a
different destination computer? *MiTM*
8. If someone installed a wireless router on your network without your knowledge, the WAP would be called *Rouge Access Point*
9. What software application can automatically ensure that your Windows-based computers have the most current security patches? *Windows Automatic Updates*
10. The two different types of virus scans are *file hashing & *

1. D
2. C
3. A
4. A
5. .
6. .
7. C
8. B
9. A
10. A
11. D
12. C
13. A
14. D
15. B
16. C
17. A
18. D
19. A
20. D

### *Questions after reading*
### *Questions*
1. __ is a group of computers connected on the Internet for the purpose of performing a task in a coordinated manner. *Botnet*
2. How often should you update your virus definitions in your antivirus software? 
3. What type of attack injects a command that overflows the amount of memory allocated and executes commands that would not normally be allowed? 
4. attacks are those that increase the effectiveness of a DoS attack. 
5. What kind of tool could a hacker use to intercept traffic on your network?
6. What type of virus uses Microsoft’s Visual Basic scripting language? 7. What is it called when someone intercepts traffic on your network that’s intended for a
different destination computer? 
8. If someone installed a wireless router on your network without your knowledge, the WAP would be called 
9. What software application can automatically ensure that your Windows-based computers have the most current security patches? 
10. The two different types of virus scans are 

1. 
2. 
3. 
4. 
5. 
6. 
7. 
8. 
9. 
10. 
11. 
12. 
13. 
14. 
15. 
16. 
17. 
18. 
19. 

Shoulder surfing - survey environment before entering data  X
Piggybacking - security  X
Tailgating - vetibules/man traps ✅
Phishing - awareness  ✅ 
Brute-force attack - strong passwords X - account lockout policy

1. Which of the following is not a technology-based attack? ✅
- [ ] A.  DoS
- [ ] B.  Ping of death
- [x] C.  Shoulder surfing
- [ ] D.  Malware
2. A command and control server is a part of which of the following attacks? ✅
- [x] A.  DDoS
- [ ] B.  Ping of death
- [ ] C.  Shoulder surfing
- [ ] D.  Malware
3. Which of the following is a DoS attack that floods its victim with spoofed broadcast
ping messages? ✅
- [ ] A.  SYN flood
- [x] B.  Smurf
- [ ] C.  Land attack
- [ ] D.  Ping of death
4. Which of the following is an attack that inundates the receiving machine with lots of packets
that cause the victim to waste resources by holding connections open? ✅
- [ ] A.  Ping of death
- [ ] B.  Zero day
- [ ] C.  Smurf
- [x] D.  SYN flood
5. In which of the following does the attacker (and his bots) send a small spoofed 8-byte UDP
packet to vulnerable NTP servers that requests a large amount of data (megabytes worth of
traffic) be sent to the DDoS’s target IP address? ✅
- [ ] A.  SYN flood
- [x] B.  NTP amplification
- [ ] C.  Smurf
- [ ] D.  DNS amplification
6. Which of the following was previously known as a man-in-the-middle attack? ✅
- [ ] A.  VLAN hopping
- [x] B.  On-path attack
- [ ] C.  LAND attack
- [ ] D.  Smurf
7. Double tagging is a part of which of the following attacks? ✅
- [x] A.  VLAN hopping
- [ ] B.  Smurf
- [ ] C.  DDoS
- [ ] D.  Malware
8. Which of the following is the process of adopting another system’s MAC address for the
purpose of receiving data meant for that system? ✅
- [ ] A.  Certificate spoofing
- [x] B.  ARP spoofing
- [ ] C.  IP spoofing
- [ ] D.  URL spoofing
9. Which of the following is connected to your wired infrastructure without your knowledge? ✅
- [x] A.  Rogue AP
- [ ] B.  Command and control server
- [ ] C.  Zombies
- [ ] D.  Botnet
10. Which of the following uses the same SSID as your AP? ✅
- [ ] A.  Rogue AP
- [ ] B.  Rogue DHCP
- [x] C.  Evil twin
- [ ] D.  Zombie

10/10
### *Resources*
[Demo of a simple DNS amplification attack](https://www.youtube.com/@MalignoAlonso)

