### *What is NAT?*
Network Address Translation (NAT) is a networking technique used for modifying IP address information in packet headers while in transit across a routing device. The primary use of NAT is to allow multiple devices on a local network to access external networks, like the internet, using a single public IP address. This not only preserves publicly routable IP addresses but also provides a layer of security by making internal network architecture invisible to external hosts. NAT can operate in various modes, including static NAT, dynamic NAT, and port address translation (PAT), each serving different needs and scenarios.
### *Configuring static NAT in PacketTracer*
To see how I could configure basic NAT translation, I have created a network scenario in PacketTracer. There is one router and a server that is acting as a service provider, this will be configured with static NAT. Here is the initial network design:
![image](https://beren-obsidian-images.imgix.net/d864c5f98cee4927abdfc3794aa7627d.png)

`Server0` has the IP address of 10.0.0.2

I will configure the `Service Provider` router first, to route all internal network packets directly out its serial interface `se2/0`:
```
Router>enable
Router#configure terminal
Router(config)#ip route 0.0.0.0 0.0.0.0 se2/0
```

This will route any packets from the `Service Provider`'s network directly out to the WAN link `se2/0` which will have one connection to our LAN. 

Configure the `Service Provider`'s router for its internal network interface `FastEthernet0/0` with a private IP address that has an 8 bit prefix length, next, enable NAT inside as it is the internal interface, lastly turn on the interface. Next, configure the external WAN interface `se2/0` with a public IP address that has an 8 bit prefix length, set a clock rate, as this is the DCE side of the router connections, then set the NAT to outside, lastly, on the router itself, configure NAT by setting the public IP to the internal network device using NAT. [[Routing commands#ip nat]]
```
##Configure internal NAT
Router(config)#interface FastEthernet0/0
Router(config-if)#ip add 10.0.0.1 255.0.0.0
Router(config-if)#ip nat inside
Router(config-if)#no shutdown
Router(config-if)#exit
##Configure external NAT
Router(config-)#interface se2/0
Router(config-if)#ip add 221.110.0.1 255.0.0.0
Router(config-if)#clock rate 64000
Router(config-if)#ip nat outside
Router(config-if)#no shutdown
Router(config-if)#exit
##Configure NAT to map to public IP
Router(config)#ip nat inside source static 10.0.0.2 221.110.0.1
```

Then finally save your configurations to [[NVRAM]] with: 
```
Router#copy rs
```

I will now configure `Router 0` for our local LAN with PAT . First, enable and configure to route all internal network packets directly out its serial interface `se2/0`:
```
Router>enable
Router#configure terminal
Router(config)#ip route 0.0.0.0 0.0.0.0 se2/0
```

Then configure internal and external NAT, and PAT, there will be no need to configure a clock rate as this is the DTE connection. For NAT translation in this step
```
##Configure internal NAT
Router(config)#interface FastEthernet0/0
Router(config-if)#ip add 192.168.1.1 255.255.255.0
Router(config-if)#ip nat inside
Router(config-if)#no shutdown
Router(config-if)#exit
##Configure external NAT
Router(config-)#interface se2/0
Router(config-if)#ip add 221.110.0.2 255.0.0.0
Router(config-if)#ip nat outside
Router(config-if)#no shutdown
Router(config-if)#exit
##Configure NAT translation
Router(config)#ip nat inside source static 10.0.0.2 221.110.0.1
```