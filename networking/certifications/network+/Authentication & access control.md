![image](https://beren-obsidian-images.imgix.net/29a2c89f86c4b60811116c82322b1f7e.png)
### *Access Control Lists (ACL)*
The primary technology that firewalls use for filtering traffic is Access Control Lists. ACLs are typically maintained through a router and filter packets based on the requesting device's source or destination IP address. A list of IP addresses to block might start with these:
- Deny any addresses from your internal networks (Prevents internal spoofing attacks).
- Deny any local host addresses (127.0.0.0/8) (Blocks loopback traffic that shouldn't be coming from external networks).
- Deny any reserved private addresses (Stops external networks from impersonating internal addresses).
- Deny any addresses in the IP multicast address range (224.0.0.0/4) (Mitigates potential for DoS attacks and conserves bandwidth).

**MAC filtering**
You can also create a MAC based ACL called MAC filtering to filter devices based on MAC addresses. MAC Filtering can be particularly useful in local networks that do not use the TCP/IP protocol stack, because it operates at the Data Link Layer (Layer 2 of the OSI model), not the Network Layer where IP operates. Key points to note about MAC filtering:
- **Protocol Agnostic**
  Since MAC filtering operates at a lower layer, it doesn't matter what upper-layer protocol you're using. It can work with any network protocol, not just TCP/IP.
- **Hardware-Specific**
  MAC addresses are hardware-specific and are unique to each network interface card (NIC), making them a somewhat reliable way to identify specific devices.
- **Complexity and Maintenance**
  MAC addresses can be harder to manage than IP addresses. MAC addresses are long, hexadecimal numbers that are not as easily readable or memorable as IP addresses.
- **Risk of Conflicts**
  Combining both IP-based and MAC-based ACLs can result in complications, potentially causing legitimate traffic to be blocked. This adds complexity to network configuration and troubleshooting.

**Port filtering**
ACLs can filter traffic based on port numbers, allowing for granular control over what type of traffic is allowed or denied. Port filtering uses an "implicit deny" approach, blocking all ports that are not explicitly opened. It's crucial to know the port numbers used by your applications in order to effectively configure the firewall. Port filtering requires manual oversight to keep track of which ports should be opened or closed based on application and security needs.

**Tunneling**
Tunneling is a security method used to protect sensitive data being sent over insecure networks like the Internet. It involves taking one protocol and encapsulating it within another. For instance, IP, commonly used for data transmission, can be wrapped within a more secure protocol like [[IPSec]]. This encapsulation encrypts the data packets, making it difficult for unauthorized users to access the information. When implemented, it essentially creates a secure "tunnel" for data transmission between two points over the Internet. All these following protocols are methods for tunneling:
- Virtual Private Network (VPN)
- Secure Sockets Layer ([[SSL]])
- Secure Sockets Layer Virtual Private Network (SSL VPN)
- Datagram Transport Layer Security ([[DTLS]])
- Layer 2 Tunneling Protocol ([[L2TP]])
- Point-to-Point Tunneling Protocol ([[PPTP - TCP 1723 & GRE 47]])
- Generic Routing Encapsulation ([[GRE]])
- Internet Protocol Security ([[IPSec]])
- ISAKMP
### *Tunneling protocols*
**VPN**
A VPN provides a secure connection that bridges a local area network (LAN) with a wide area network (WAN), making the remote network appear as a local resource. 

Various types of VPNs serve different purposes:
- **Client-to-Site (Remote-Access) VPN**
  Enable remote workers to securely access the corporate network.
```
# Server Configuration
openvpn --config server.conf

# Client Configuration
openvpn --config client.conf  
```
- **Host-to-Host VPN**
  Secure traffic between two specific hosts, from the network interface card (NIC) of one to the NIC of the other.
```
# Host A Configuration
openvpn --remote [Host B IP] --dev tun --ifconfig [Host A tun IP] [Host B tun IP] --secret static.key

# Host B Configuration
openvpn --remote [Host A IP] --dev tun --ifconfig [Host B tun IP] [Host A tun IP] --secret static.key
```
- **Site-to-Site VPN**
  Connect remote sites to a company's main office, using the internet as a medium instead of expensive WAN links.
```
# Site A Configuration
openvpn --config siteA.conf

# Site B Configuration
openvpn --config siteB.conf
```
- **Extranet VPN**
  Facilitate secure business-to-business communications, allowing limited network access to suppliers, partners, and customers.
```
# Corporate Server Configuration
openvpn --config corporate-server.conf

# Extranet Client Configuration
openvpn --config extranet-client.conf
```

**SSL & SSL VPN**
[[SSL]] (Secure Socket Layer) was developed by Netscape for secure web communication, it uses RSA public-key encryption. SSL eventually merged with other Transport layer protocols to form Transport Layer Security (TLS). SSL is generally used for securing individual connections between a client and a server. An SSL VPN is used for creating a secure tunnel through which multiple services can pass.

**DTLS**
[[DTLS]] (Datagram Transport Layer Security) is a security protocol that adapts the features of the stream-based TLS protocol for use with datagram-based applications. It aims to offer the same level of security as TLS, including protection against unauthorized access, data manipulation, and false messaging. DTLS is commonly used in scenarios where data is sent in discrete packets rather than a continuous stream, such as VoIP or IoT applications.

**L2TP**
Layer 2 Tunneling Protocol ([[L2TP]]) is a combination of Microsoft’s Point-to-Point Tunneling Protocol (PPTP) and Cisco’s Layer 2 Forwarding (L2F). Not limited to TCP/IP; also supports a range of other protocols. It is particularly useful for connecting non-TCP/IP networks over the Internet.  

L2TP (Layer 2 Tunneling Protocol) itself does not provide encryption. It is commonly paired with IPsec (Internet Protocol Security) to add a layer of encryption and security. When used in conjunction with IPsec, data is encrypted using algorithms like AES or 3DES. This pairing is often referred to as L2TP/IPsec.

**PPTP**
Point-to-Point Tunneling Protocol ([[PPTP - TCP 1723 & GRE 47]]) is an older VPN protocol developed by Microsoft and other companies. It combines unsecured and secured sessions, which can cause issues with routers. Initially popular for its use in dial-up networking, it has largely been phased out due to security vulnerabilities and has been replaced by more secure options like L2TP and IPsec.

**GRE**
Generic Routing Encapsulation ([[GRE]]) is a tunneling protocol designed to package different kinds of network protocols within IP tunnels. This means it can carry data from various protocols like EIGRP, OSPF, RIP or IPv6 through an IP network. When it comes to its structure, a GRE tunnel includes headers for the original data (referred to as the passenger protocol), the GRE protocol itself, and the protocol used to deliver the data (usually IP).

**IPSec**
[[IPSec]] was designed for providing authentication and encryption over the internet. It operates at layer 3, and secures all applications that operate in the layers above it. IPSec supports both IPv4 and IPv6 and it is the standard for VPNs on the internet.

**ISAKMP**
Internet Security Association And Key Management Protocol ([[ISAKMP]]) is a protocol used to establish secure channels by negotiating and managing Security Associations (SAs). It operates independently of specific encryption or authentication methods, offering a flexible framework commonly used with IPSec for secure communications.
### *Remote access*
**RAS**
Remote Access Services (RAS) is not a protocol but a combination of hardware and software for remote-access connection. The term RAS was popularized by Microsoft, especially in the context of its Windows NT-based remote-access tools.

**RDP**
Remote Desktop Protocol ([[RDP]]) is a remote management software that operates over TCP port 3389 and allows remote access to a device. Upon connection, users see a terminal window that mimics the interface of Windows or another OS, allowing them to access remote files and applications. It offers 128-bit encryption using the RC4 encryption algorithm and also supports TLS 1.0.

**PPP**
Point-to-Point Protocol ([[PPP]]) serves as a widely accepted standard for transferring data across serial connections. It's the go-to protocol for Internet Service Providers for facilitating network access to individual computers.

**PPPoE**
PPPoE (Point-to-Point Protocol over Ethernet) extends the Point-to-Point Protocol (PPP) to operate over Ethernet networks. It encapsulates PPP frames within Ethernet frames, enabling authentication, encryption, and compression features of PPP over an Ethernet network. It was developed to handle the surge in high-speed internet connections like ADSL and cable modems. Often integrated with a Remote Authentication Dial In User Service (RADIUS) server to manage connections and offer enhanced security features such as authentication and encryption.

PPPoE has two operations:
- Discovery Phase: The MAC addresses of both endpoints are exchanged. A session ID is also generated.
- Session Phase: A point-to-point connection is established using the information gathered during the discovery phase, and data transmission occurs.

**ICA**
ICA (Independent Computing Architecture) is a protocol developed by Citrix Systems for server-client communication. The most common application using ICA is Citrix's WinFrame. WinFrame allows administrators to set up Windows applications on a Windows server. Provides network administrators with flexibility in terms of client operating systems. A downside is that these connections can be slow due to the high amount of translation required for proper communication between client and server.

**SSH**
[[SSH - 22 TCP]] (Secure Shell) was designed for secure communication as alternatives to Telnet which send requests and responses in plain text. SSH utilizes public-key cryptography for the authentication of the remote computer and potentially the user. The public key is placed on any computer that needs to allow access to the owner of the corresponding private key.

**Out-of-Band Management**
Out-of-band management is a method of managing servers that do not use the main network. Integrated Lights-Out (iLO) is an example of this technology, specifically designed for HP servers. The physical connection for iLO is usually an Ethernet port labeled "ILO" on the server. iLO allows complete access to the server regardless of its operational state, without requiring additional software installation

**VNC**
VNC (Virtual Network Connection) is similar to RDP, it uses the RFP (Remote Frame Buffer) protocol which allows remote access to graphical user interfaces. VNC uses a server to run on the machine sharing its GUI, and the client requires software to access that shared GUI.

>One big difference in VNC and RDP is that VNC sends raw pixel data (which does make it work on any desktop type) while RDP uses graphic primitives or higher-level commands for the screen transfer process.
### *Authentication*
**Local authentication**
Local authentication is when users authenticate directly to their computer, rather than to a network or domain. During local authentication, the computer verifies the user's account and password against a local database. In Windows, this local user database is called the Security Accounts Manager (SAM), SAM is located in the `C:\windows\system32\config\` directory. In Linux systems, the local user database is a text file located at `/etc/passwd`, the `/etc/passwd` file lists all valid usernames along with associated information.

**LDAP**
A directory service is essentially a centralized database for managing network-related data.
Lightweight Directory Access Protocol ([[LDAP - 389 TCP]]) is a widely-used standard for directory services. LDAP is based on the older X.500 standard, which uses Directory Access Protocol (DAP). In X.500, a distinguished name (DN) provides the full path to an entry, while the relative distinguished name (RDN) provides the entry name without the full path. LDAP operates in a client-server architecture, using TCP port 389 for communication.
For advanced security, LDAP can operate over SSL, using TCP port 636.

**Certificates**
A digital certificate provides credentials to prove the identity of an entity, typically a user, and associates it with a public key. The minimum fields a digital certificate should have are the serial number, the issuer, the subject (owner), and the public key. An X.509 certificate complies with the X.509 standard and contains additional fields like Version, Algorithm ID, Validity, Subject Public Key Info, and optional fields like Issuer and Subject Unique Identifiers and Extensions. 

VeriSign introduced different classes of digital certificates:
- Class 1: Aimed at individuals, mainly for email purposes. These certificates are saved by web browsers.
- Class 2: Designed for organizations that need to provide proof of identity.
- Class 3: Used for servers and software signing. These certificates require independent verification and identity checks by the issuing Certificate Authority (CA).

**Public Key Infrastructure**
PKI is a framework that attaches individuals to cryptographic key pairs. PKIPKI provides a layer of trust so people can be confident in the identity of who they're conversing with online. 

**Kerberos**
Kerberos is comprehensive security system. The system operates by issuing tickets to authenticated users, these tickets are short-lived but get automatically refreshed as long as the user remains logged in. Synchronized clocks across all systems in a Kerberos domain are crucial, Windows does this automatically. Redundant servers are vital as all authentication would be inoperative if a single Kerberos server goes down. A centralized database stores all users' secret keys, making security highly dependent on this database.

Initially, a user logs into a system running Kerberos and requests a Ticket Granting Ticket (TGT). The TGT is returned by the authentication service. Using the TGT, the user requests an application ticket. The application ticket is returned by the ticket-granting service. Finally, the user can request a particular service, authenticating themselves with the application ticket.

**AAA & AAAA**
AAA stands for Authentication, Authorization, and Accounting. AAAA is an extended version, adding Auditing to the original three A's. Neither AAA nor AAAA are protocols; they are organized frameworks for network security management. These frameworks aim to manage network security from a single, centralized location. RADIUS and TACACS+ are two commonly used technologies that implement the AAA model.

**RADIUS**
RADIUS (Remote Authentication Dial In User Service) originally started as a dial-up verification service. 
- Used for authentication and accounting across various types of links, including dial-up.
- Stored usernames and passwords centrally.
- Can be used in firewalls to authenticate users trying to access specific TCP/IP ports.
- Operates on a client-server model.
- Authentication server that allows for domain-level authentication on both wired and wireless networks

**TACACS+**
TACACS+ (Terminal Access Controller Access-Control System Plus) is another AAA method and serves as an alternative to RADIUS.
- Unlike RADIUS, TACACS+ separates user authentication and authorization.
- Uses the TCP protocol, making it generally more stable and secure compared to RADIUS, which uses UDP.
- Capable of working with multiple wireless APs, RAS servers, or LAN switches that are 802.1X capable.
- 
The flow of TACACS+ can be broken down into several steps that encompass authentication, authorization, and accounting:
- **Login Request**: When a user tries to access a network resource, a login request is initiated. This request includes the user's credentials like username and password.
- **Authentication**: The client device contacts the TACACS+ server to authenticate the user. The server checks the credentials against its database.
- **Reply Success**: If the credentials are valid, the server replies with a success message, indicating that the user has been authenticated.
- **Authorization Request**: After successful authentication, the TACACS+ server then receives an authorization request for the user to gain specific access rights.
- **Reply Success**: Upon verification, the server replies with another success message, granting the user the necessary authorizations.
- **Accounting Start**: At this point, the accounting aspect of the AAA model kicks in. The TACACS+ server starts tracking user activities such as login time and data usage.
- **Reply Success**: The server acknowledges the start of the accounting process with a success message, confirming that the user's activities will be tracked.
- **User Session**: The user can now use the network resources based on the granted authorizations.
- **Logout Request**: When the user decides to logout or the session ends, a logout request is sent to the TACACS+ server.
- **Accounting Stop**: The TACACS+ server logs details about the session, like the total time the user was logged in, the number of bytes and packets sent and received during the session, the reason for disconnection, and stops the accounting process.

**Unified voice services**
Integration of voice, data, and video traffic over IP networks. Examples include VoIP and video streaming. These services often use Quality of Service (QoS) mechanisms to prioritize latency-sensitive traffic. Unified voice can also integrate with messaging systems like IM, presence, and email, including voicemail.

**Network Controllers**
Refers to network interface cards (NICs) that can handle both LAN and SAN traffic. These NICs usually offload processing from the CPU and some can perform encryption. The term can also describe devices that control admission or access to a network, ensuring systems are safe and secure before accessing the network.

**Network Access Control**
is a specific method or protocol within the broader scope of network access control systems. It focuses primarily on securing hosts before they can access a network. NAC is often used in wireless networks and utilizes IEEE 802.1X for authentication.

**IEEE 802.1X**
IEEE 802.1X is an open framework standard used by NAC for authentication. The client (supplicant) asks the access point (authenticator) to join the network and provides credentials. The authenticator forwards these to a centralized authentication server, which then sends back an accept or reject message. The access point then allows or denies network access based on this message.

**Challenge Handshake Authentication Protocol**
Challenge Handshake Authentication Protocol (CHAP) is a secure authentication protocol that never sends the username and password across the network. Instead, it uses a shared secret configured on both the client and server.

Authentication Process:
- The client sends an authentication request.
- The server responds with a nonce and an ID value.
- Both the client and the server generate a hash value using MD5 encryption algorithm, which incorporates the shared secret, nonce, and ID.
- The hash value from the client is sent back to the server.
- The server compares the received hash value with its own calculated hash value.
- If they match, the client is authenticated.

Both the client and server are configured with this text phrase that is known to both but never crosses the wire. Random values sent by the server to the client, used along with the shared secret to generate the hash. MD5 encryption is used to generate a one-way hash value. CHAP has replaced Password Authentication Protocol (PAP) because PAP sends usernames and passwords in clear text, whereas CHAP does not.

**Network Access Control Systems**
These are overarching systems aimed at controlling device access based on their security posture. Systems like Cisco's Network Admission Control (NAC) and Microsoft’s Network Policy and Access Services (NPAS) fall under this category. They may check a device's OS updates and anti-malware status before granting network access and can sometimes remediate issues before allowing access.
### *Questions*
1. What are two possible items checked during a posture assessment? **Windows registry settings and os updates anti malware updates** Y
2. Which type of agent is one that is installed on a NAC client and starts when the operating system loads? **Persistent** Y
3. Which encryption protocol or standard allows you to create a private network on an intranet? VPN 1/2
4. Which user-authentication method uses a public key and private key pair? PKI,  asymmetric Y
5. In an authentication system that uses private and public keys, who should have access to the private key? The authenticating system/server admin. 1/2
6. Which authentication method relies on tickets to grant access to resources? Kerberos Y
7. In computer security, what does AAA stand for? Authentication, Authorization, Accounting 1/2
8. Which network access security method is commonly used in wireless networks? 802.1X Y
9. Which user-authentication method is available only in an all-Windows environment? MS-CHAP Y
10. Which user-authentication method utilizes the TCP protocol? TACACS+ Y
8.5/10

1. Nonpersistent or dissolvable NAC agents may help to make what possible?  A. Y
2. What is the main difference between a private network and a public network? C. Y
3. You have a remote user who can connect to the Internet but not to the office via their VPN client. After determining the problem, which should be your next step? B y
4. Which IP address should you deny into your internetwork? D Y
5. Which of the following is a tunneling protocol? D Y
6. Which tunneling protocol is based on RSA public-key encryption? A Y
7. What is the minimum number of characters you should use when creating a secure
password? C Y
8. In which layer of the OSI model does IPSec operate? B Y
9. Which protocol works in both the transport mode and tunneling mode? B X
10. Companies that want to ensure that their data is secure during transit should use which of the following? B Y
11. Which network utilities do not have the ability to encrypt passwords? (Select two.) A,C Y
12. To encode or read an encrypted message, what tool is necessary? C Y
13. Which of the following is not an enhancement provided by TLS version 2.0? C Y
14. Which of the following is not a type of public-key encryption? D Y
15. Which of the following VPN protocols runs over TCP port 1723, allows encryption to be
done at the data level, and allows secure access? D 1/2
16. At which stage of PPPoE are the MAC addresses of the endpoints exchanged? B Y
17. When utilizing multifactor authentication, which of the following is an example of
verifying something you are? C Y
18. Which of the following authentication methods allows for domain authentication on both
wired and wireless networks? C X
19. Which user-client-server authentication software system combines user authentication and authorization into one central database and maintains user profiles? A Y
20. Which of the following is not a Network Access Control method? D Y
18/20

**Notes**
- IPSec works in both transport mode and tunneling mode. In transport mode, a secure IP connection between two hosts is created. Data is protected by authentication or encryption (or both). Tunnel mode is used between network endpoints to protect all data going through the tunnel.
- PPTP is a VPN protocol that was created by Microsoft and uses TCP port 1723 for
authetication and Generic Routing Encapsulation (GRE) to encrpyt data at the
Application level.
- RADIUS servers provide both authentication and encryption services and can combine
these into one service. RADIUS can be used for allowing or denying both wired and
wireless access at the domain level.