### *ip route*
The `ip route` command in Cisco IOS is used to manually define static routes in the routing table. Static routes are often used for small networks, specific paths that need to be prioritized, or as a backup for dynamic routing protocols. The command provides a way to specify how to reach a particular destination network.
```
ip route [destination_network] [subnet_mask] {[next_hop_address] | [exit_interface]} [administrative_distance] [permanent]
```

- `destination_network`: The IP address of the network you want to route to.
- `subnet_mask`: The subnet mask for the destination network.
- `next_hop_address`: The IP address of the next router or hop that will get the packet closer to the destination.
- `exit_interface`: The local interface through which the packets will be sent to reach the destination.
- `administrative_distance`: Optional. A value between 0 and 255 representing the trustworthiness of the route. Lower values are more trusted.
- `permanent`: Optional. Makes the route permanent, meaning it will not be removed if the router reboots.

Here are some examples of the `ip route` command.
**Route to a specific network via a next-hop address**:
```
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

This tells the router to forward packets destined for the 192.168.2.0/24 network to the next hop at 10.0.0.2.

**Route to a specific network via an exit interface**:
```
ip route 192.168.2.0 255.255.255.0 FastEthernet0/0
```

This instructs the router to use its FastEthernet0/0 interface to reach the 192.168.2.0/24 network.

**Route with administrative distance**:
```
ip route 192.168.2.0 255.255.255.0 10.0.0.2 120
```

In this example, the route to 192.168.2.0/24 via 10.0.0.2 has an administrative distance of 120.
### ip nat
The `ip nat inside` and `ip nat outside` commands are used in Cisco routers to define which interfaces participate in Network Address Translation (NAT) and in what capacity. These commands are crucial for configuring NAT, which allows for the translation of private IP addresses to a public IP address and vice versa.

`ip nat inside`:
- **Command Placement**: This command is applied to the interface that is connected to the internal network—the LAN segment that needs to have its private IP addresses translated.
- **Function**: When a packet leaves the internal network and goes to the external network, the router looks for this "inside" designation to perform the NAT operation. The source IP address is changed from a private to a public IP address based on the NAT configuration.

`ip nat outside`
- **Command Placement**: This command is applied to the interface facing the external network, usually the interface connected to the Internet Service Provider (ISP) or a WAN link.
- **Function**: When a packet comes from the external network destined for the internal network, the router looks for this "outside" designation to perform the reverse NAT operation. The destination IP address is changed from the public IP back to the appropriate private IP address.

To configure a router to use NAT, you can configure it with these commands:
```
##Configuring the inside NAT interface
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 exit

##Configuring the outside NAT interface
interface FastEthernet0/1
 ip address 203.0.113.1 255.255.255.252
 ip nat outside
 exit

##Configuring NAT translation
ip nat inside source list 1 interface FastEthernet0/1 overload
access-list 1 permit 192.168.1.0 0.0.0.255
```

- **Inside NAT Interface**: The first set of commands configures the interface facing your internal network (FastEthernet0/0) and sets it as the "inside" NAT interface.
    
- **Outside NAT Interface**: The second set of commands configures the interface facing the external network (FastEthernet0/1) and designates it as the "outside" NAT interface.
    
- **NAT Translation**: The last set of commands actually sets up the NAT translation rules. It specifies that traffic from the 192.168.1.0/24 network should be translated when going out of the FastEthernet0/1 interface. The `overload` keyword allows for multiple internal addresses to be mapped to the single external address (PAT, or Port Address Translation).