![image](https://beren-obsidian-images.imgix.net/f92d91b073901a24f5eaed76e60dece5.png)
### *Temperatures*
When CPUs get overheated, a cycle of reboots can ensue. Keep the ambient air within normal ranges (approximately 15.6°C to 32°C) and at a constant temperature. Leaving slots on the back of the computer open alters the air circulation and causes more dust to be pulled into the system.
### *CPU*
These are the standard terms used for analyzing CPU usage:
- **Processor% Processor Time:** This is a universal counter, indicating how much of the CPU's capacity is being used.
- **Processor% User Time:** Shows the time CPU spends processing user-initiated tasks, which varies depending on the server's role.
- **Processor% Interrupt Time:** Important for understanding how much time is spent handling hardware requests.
- **System\Processor Queue Length:** Indicates if the CPU is able to keep up with the workload, which can be particularly relevant for servers handling a lot of concurrent tasks or requests.
### *Memory*
You should understand these concepts before trying to read memory usage statistics:
**Memory Leaks**
- A memory leak in an operating system (OS) context usually refers to a situation where a program or process incorrectly manages memory allocations. Instead of releasing memory that it no longer needs, the program retains it unnecessarily. Over time, this can lead to a significant portion of the system's memory being unavailable for other processes.
- Memory leaks often occur in programs due to bugs or errors in code. They're less common in the OS itself, but can still happen, especially in complex systems with many interacting components.

**Page table entries**
- A page table is part of the OS's virtual memory management system. It keeps track of the relationship between virtual addresses (used by programs) and physical addresses (in the RAM).
- Each entry in a page table corresponds to a "page" of memory. The OS uses these entries to translate virtual memory addresses to physical addresses.
- "Free System Page Table Entries" refers to the number of entries in this table that are not currently being used to map virtual pages to physical memory.

**Paged pool**
- The paged pool is a region of system memory (RAM) in Windows operating systems used by the kernel and operating system drivers to store objects that can be written to disk (paged out) when not in use. This is in contrast to the non-paged pool, which consists of memory that must always stay in physical RAM and cannot be paged out.
- Objects in the paged pool can be moved to the page file on the hard disk to free up physical RAM for other uses when they are not actively being accessed or used.

These are the standard terms used for analyzing memory usage:
- **Memory\% Committed Bytes in Use**: This is the amount of virtual memory in use. Anything over 80% indicates you need more memory.
- **Memory\Available Mbytes**: This is the amount of physical ram you have access to. Anything less than 5% means you need more memory.
- **Memory\Free System Page Table Entries**: The number of entries in the page table not currently in use by the system. If the number is less than 5000, there could be a memory leak (memory is not being freed by an application).
- **Memory\Pool Non-Paged Bytes:** The size, in bytes, of the non-paged pool, which contains memory that cannot be paged to the disk. If the value is greater than 175MB (an application is not releasing its allocated memory when it is done).
- **Memory\Pool Paged Bytes:** The size, in bytes, of the paged pool which contains objects that can be pages to disk. (If this value is greater than 250MB there may be a memory leak)
- **Memory\Pages per Second:** The rate at which pages are written to and read from the disk during paging. If the value is greater than 1000, as a result of excessive paging, there may be a memory leak.
### *Networking*
- **Network Interface% Bytes Total/Sec:** This is the percentage of bandwidth the NIC is capable of that is currently being used. If this is more than 70%, the interface is saturated or not keeping up.
- **Network Interface% Output Queue Length:** The number of packets in the output queue. If this value is over 2, the NIC is not keeping up with the workload.

**Latency**
Measuring latency is typically done using a metric called round-trip time (RTT). This metric is calculated using a ping, a command-line tool that bounces a user request off a server and calculates how long it takes to return to the user device.
### *Network device logs*
It is important to have a baseline of what your network looks like day-to-day, so you can determine what could be happening when problems arise in the network.
#### Audit logs
Audit logs record user activities and system events. Windows Server uses Event Viewer for accessing various logs. Types of windows logs in event viewer will include:
- **Application Log**: Records events from applications, like updates or operational issues in software such as Microsoft Office.
- **Security Log**: Logs security-related events, including successful or failed login attempts and potential security breaches.
- **System Log**: Tracks events from Windows system components like drivers and services, indicating their operational status.

In a Linux OS, you would typically use an application like auditd (Audit Daemon), it is a tool used for security auditing, It tracks security-relevant information based on pre-defined rules. This includes file accesses, modifications, and system calls. The logs generated by auditd are detailed and can be used for deep analysis of system activities, security investigations, and compliance with security policies. Admins can configure `auditd` to monitor specific activities and system changes, tailoring it to the security needs of the Linux system.

These are both essential tools to monitor, troubleshoot, and ensure the integrity of their systems.
#### Syslog
Syslog servers communicate with routers using either UDP or TCP. Routers send messages to the syslog server as events occur, rather than at regular intervals. The frequency and type of messages sent can be configured on the router based on the needs of the network and the capacity of the syslog server.

These four examples are popular ways to gather messages from Cisco devices:
- Logging buffer (on by default)
- Console line (on by default)
- Terminal lines (using the terminal monitor command)
- Syslog server
### *Interface statistics*
```
0 packets input, 0 bytes, 0 no buffer:

Packets Input: The number of packets received on the interface. In this case, zero packets have been received.
Bytes: The total amount of data in bytes received. Here, it's zero bytes.
No Buffer: Indicates the number of packets dropped because there was no buffer space in the device's memory. Zero means no packets were dropped for this reason.
Received 0 broadcasts, 0 runts, 0 giants, 0 throttles:

Broadcasts: The count of broadcast packets received. Broadcast packets are sent to all nodes on the network.
Runts: Packets that are smaller than the minimum Ethernet frame size. They are usually a sign of collisions or other errors.
Giants: Packets that exceed the maximum Ethernet frame size, indicating a problem.
Throttles: The number of times the receiver on the port has been paused due to congestion or overflow.
0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort:

Input Errors: Total number of errors on incoming packets.
CRC: Cyclic Redundancy Check errors, indicating a problem in the network (e.g., noise, collision).
Frame: Framing errors, usually caused by incorrect serialization of bits onto the wire.
Overrun: Occurs when the data surpasses the buffer's capacity.
Ignored: Packets ignored due to the processor being overwhelmed.
Abort: Errors causing packet abortion.
0 input packets with dribble condition detected:

Dribble Condition Detected: A dribble bit error indicates that a packet is slightly longer than expected. Zero means no such errors were detected.
0 packets output, 0 bytes, 0 underruns:

Packets Output: The number of packets sent out of the interface.
Bytes: The total amount of data in bytes sent.
Underruns: Occurs when the transmit buffer is emptied before the complete frame is sent, indicating a transmission problem.
0 output errors, 0 collisions, 1 interface resets:

Output Errors: Total errors while transmitting packets.
Collisions: Occurs in half-duplex modes when two devices transmit simultaneously.
Interface Resets: The number of times the interface has been reset. Here, there's been one reset.
0 babbles, 0 late collision, 0 deferred:

Babbles: Transmit errors due to packets longer than the allowed maximum.
Late Collision: A collision occurring later than expected in the transmission process, indicating distance or timing issues.
Deferred: Packets delayed because the medium was busy.
```
### *Questions before reading chapter*
> All questions are marked after completing the chapter. 

1. Which of the following represents the percentage of up time provided when four nines of fault tolerance are present?
- [ ] A. 9.999 percent
- [x] B. 99.99 percent
- [ ] C. 99.999 percent
- [ ] D. 90.9 percent
2. Which of the following is not a commonly used NetFlow identifier?
- [ ] A. Source IP
- [ ] B. Destination port number
- [ ] C. Layer 2 protocol field
- [x] D. Input logical interface
3. Which service can identify major users of the network, meaning top talkers?
- [ ] A. Syslog
- [x] B. SIEM
- [ ] C. NetFlow
- [ ] D. SNMP
4. Which of the following refers to the standard level of performance of a certain device or to the normal operating capacity for your whole network?
- [x] A. Baseline
- [ ] B. Target
- [ ] C. Normal
- [ ] D. Utilization
5. Raised floors are used to address which of the following?
- [ ] A. Electrical issues
- [x] B. Flooding
- [ ] C. Terrorism
- [ ] D. Theft
6. Which of the following removes the UPS from between the device and the wall output conceptually, without disconnecting it?
- [ ] A. Inline mode
- [ ] B. Offline mode
- [ ] C. Bypass mode
- [x] D. Maintenance mode
7. Which UPS value assumes that all of the attached devices are pulling the maximum
amount of power?
- [ ] A. Maximum load
- [ ] B. Volts ampere
- [ ] C. UPC
- [x] D. Capacity
8. Which value is the amount of time a UPS can operate based on the current battery charge?
- [ ] A. Runtime
- [x] B. Remaining life
- [ ] C. Lifetime
- [ ] D. Live time
9. The proper shutdown of a system is called which of the following?
- [ ] A. Stateful
- [x] B. Graceful
- [ ] C. Stateless
- [ ] D. Quick
10. Which of the following is the maximum amount of power the UPS can supply at any
moment in time?
- [x] A. Maximum load
- [ ] B. Volts ampere
- [ ] C. UPC
- [ ] D. Capacity
11. What devices have a battery attached that can provide power to the devices in the event of a power outage?
- [ ] A. NFC
- [ ] B. VA
- [x] C. UPS
- [ ] D. Syslog server
12. Which condition leads to shorts?
- [ ] A. High temperature
- [ ] B. High humidity
- [x] C. Low temperature
- [ ] D. Low humidity
13. Damage from static electricity can occur when which of the following is present?
- [ ] A. High temperature
- [x] B. High humidity
- [ ] C. Low temperature
- [ ] D. Low humidity
14. Which of the following causes system reboots?
- [x] A. High temperature
- [ ] B. High humidity
- [ ] C. Low temperature
- [ ] D. Low humidity
15. A humidifying system should be used to maintain the level above what percent?
- [ ] A. 30 percent
- [ ] B. 40 percent
- [x] C. 50 percent
- [ ] D. 60 percent
16. Which error message indicates that the router has a layer 3 packet to forward and is lacking some element of the layer 2 header that it needs to be able to forward the packet toward the next hop?
- [x] A. CRC error
- [ ] B. Encapsulación error
- [ ] C. Duplex mismatch
- [ ] D. Speed mismatch
17. Any Ethernet packet that is greater than 1518 bytes is which of the following?
- [x] A. Giant
- [ ] B. Runt
- [ ] C. Outlier
- [ ] D. Exception
18. Using a cable that is too long can result in which if the following?
- [ ] A. Runt
- [ ] B. Giant
- [ ] C. Collisions
- [x] D. CRC errors
19. Which of the following means that packets have been damaged?
- [ ] A. Runt
- [ ] B. Giant
- [ ] C. Collisions
- [x] D. CRC errors
20. If you have a duplex mismatch, which counter will increment?
- [ ] A. Late collisions
- [ ] B. Babbles
- [x] C. Watchdog
- [ ] D. Unknown protocol drops

### *Questions after reading the chapter*
The percentage of time the CPU spends executing a non idle thread. Processor% Processor Time ✅

The amount of physical memory in megabytes currently available. Memory% MB/available X  Memory\Available Mbytes

The percentage of bandwidth the NIC is capable of that is currently being used. Network interface% Bytes/total sec X Network Interface\Bytes Total/Sec

The delay typically incurred in the processing of network data. Network interface% Que time/length X Latency

Occurs when the data flow in a connection is not consistent; that is, it increases and decreases in no discernible pattern. Jitter ✅

Supports plaintext authentication with MD5 or SHA with no encryption but provides GET BULK. SNMPv2 ✅

Sent by SNMP agents to the NMS if a problem occurs. Trap ✅

Identifier mechanism standardized by the International Telecommunications Union (ITU) and ISO/IEC for naming any object, concept, or “thing” with a globally unambiguous persistent name. ObjectID ✅

Hierarchical structure into which SNMP OIDs are organized. Management Information Base ✅

Refers to the standard level of performance of a certain device or to the normal operating capacity for your whole network. Baseline ✅

Centralizes and stores log messages and can even timestamp and sequence them. Syslog ✅

Provides real-time analysis of security alerts generated by network hardware and applications. SIEM ✅

Errors that mean packets have been damaged. CRC ✅

8.5/10

1. Which of the following represents the percentage of up time provided when four nines of fault tolerance are present? ✅
- [ ] A. 9.999 percent
- [x] B. 99.99 percent
- [ ] C. 99.999 percent
- [ ] D. 90.9 percent
2. Which of the following is not a commonly used NetFlow identifier? ✅
- [ ] A. Source IP
- [ ] B. Destination port number
- [x] C. Layer 2 protocol field
- [ ] D. Input logical interface
3. Which service can identify major users of the network, meaning top talkers? ✅
- [ ] A. Syslog
- [ ] B. SIEM
- [x] C. NetFlow
- [ ] D. SNMP
4. Which of the following refers to the standard level of performance of a certain device or to the normal operating capacity for your whole network? ✅
- [x] A. Baseline
- [ ] B. Target
- [ ] C. Normal
- [ ] D. Utilization
5. Raised floors are used to address which of the following? ✅
- [ ] A. Electrical issues
- [x] B. Flooding
- [ ] C. Terrorism
- [ ] D. Theft
6. Which of the following removes the UPS from between the device and the wall output conceptually, without disconnecting it? ✅
- [ ] A. Inline mode
- [ ] B. Offline mode
- [x] C. Bypass mode
- [ ] D. Maintenance mode
7. Which UPS value assumes that all of the attached devices are pulling the maximum
amount of power? X Capacity
- [x] A. Maximum load
- [ ] B. Volts ampere
- [ ] C. UPC
- [ ] D. Capacity
8. Which value is the amount of time a UPS can operate based on the current battery charge? ✅
- [x] A. Runtime
- [ ] B. Remaining life
- [ ] C. Lifetime
- [ ] D. Live time
9. The proper shutdown of a system is called which of the following? ✅
- [ ] A. Stateful
- [x] B. Graceful
- [ ] C. Stateless
- [ ] D. Quick
10. Which of the following is the maximum amount of power the UPS can supply at any
moment in time? X Capacity
- [ ] A. Maximum load
- [x] B. Volts ampere
- [ ] C. UPC
- [ ] D. Capacity
11. What devices have a battery attached that can provide power to the devices in the event of a power outage? ✅
- [ ] A. NFC
- [ ] B. VA
- [x] C. UPS
- [ ] D. Syslog server
12. Which condition leads to shorts? X High humidity
- [x] A. High temperature
- [ ] B. High humidity
- [ ] C. Low temperature
- [ ] D. Low humidity
13. Damage from static electricity can occur when which of the following is present? X Low humidity
- [ ] A. High temperature
- [x] B. High humidity
- [ ] C. Low temperature
- [ ] D. Low humidity
14. Which of the following causes system reboots? ✅
- [x] A. High temperature
- [ ] B. High humidity
- [ ] C. Low temperature
- [ ] D. Low humidity
15. A humidifying system should be used to maintain the level above what percent? ✅
- [ ] A. 30 percent
- [ ] B. 40 percent
- [x] C. 50 percent
- [ ] D. 60 percent
16. Which error message indicates that the router has a layer 3 packet to forward and is lacking some element of the layer 2 header that it needs to be able to forward the packet toward the next hop? ✅
- [ ] A. CRC error
- [x] B. Encapsulación error
- [ ] C. Duplex mismatch
- [ ] D. Speed mismatch
17. Any Ethernet packet that is greater than 1518 bytes is which of the following? ✅
- [x] A. Giant
- [ ] B. Runt
- [ ] C. Outlier
- [ ] D. Exception
18. Using a cable that is too long can result in which if the following? X Collisions
- [ ] A. Runt
- [ ] B. Giant
- [ ] C. Collisions
- [x] D. CRC errors
19. Which of the following means that packets have been damaged? ✅
- [ ] A. Runt
- [ ] B. Giant
- [ ] C. Collisions
- [x] D. CRC errors
20. If you have a duplex mismatch, which counter will increment? ✅
- [x] A. Late collisions
- [ ] B. Babbles
- [ ] C. Watchdog
- [ ] D. Unknown protocol drops

15/20
### *Resources*
[pushing-the-limits-of-windows-paged-and-nonpaged-pool](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-paged-and-nonpaged-pool/ba-p/723789)
[Pushing the Limits of Windows: Physical Memory](http://blogs.technet.com/markrussinovich/archive/2008/07/21/3092070.aspx)
[Pushing the Limits of Windows: Virtual Memory](http://blogs.technet.com/markrussinovich/archive/2008/11/17/3155406.aspx)
[Pushing the Limits of Windows: Paged and Nonpaged Pool](http://blogs.technet.com/markrussinovich/archive/2009/03/26/3211216.aspx)
[Pushing the Limits of Windows: Processes and Threads](http://blogs.technet.com/markrussinovich/archive/2009/07/08/3261309.aspx)
[Pushing the Limits of Windows: Handles](http://blogs.technet.com/markrussinovich/archive/2009/09/29/3283844.aspx)
[Pushing the Limits of Windows: USER and GDI Objects – Part 1](http://blogs.technet.com/markrussinovich/archive/2010/02/24/3315174.aspx)
[Pushing the Limits of Windows: USER and GDI Objects – Part 2](http://blogs.technet.com/markrussinovich/archive/2010/03/31/3322423.aspx)