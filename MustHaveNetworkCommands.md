## Table Of Content
- [IPTables is a saviour](#iptables-is-a-saviour)
- [Route commands](#route-commands)
- [Show cgroups](#show-cgroups)
- [Some More Terms Used in Networking](#Some-More-Terms-Used-in-Networking)
- [Most used Network OSI Layers and their Usages](#Most-used-Network-OSI-Layers-and-their-Usages)

#### IPTables is a saviour
```sh
iptables -vL -t filter
iptables -vL -t nat
iptables -vL -t mangle
iptables -vL -t raw
iptables -vL -t security
```

#### Route commands
```sh
route (should be replaced by ip -r)
netstat -tlpn
ip route list
ipset list
tcpdump
ip xfrm <command>
brctl
brctl showmacs <bridge network name>
bridge
```

#### Show cgroups
```sh
cat /proc/cgroups
```

### Some More Terms Used in Networking
NAT -- Network Address TRanslation
SNAT -- Source IP Address NAT
DNAT -- Destination IP Address NAT
netns -- Network Namespaces
netfilter -- A Kernel Module which applies firlter on routing rules. It has a User Mode utility iptables to configura this
BFP -- Berkley Packet Filter (A new Linux kernel technology, which enables the dynamic insertion of powerful security visibility and control logic within Linux itself)
BGP -- BGP (Border Gateway Protocol) is protocol that manages how packets are routed across the internet through the exchange of routing and reachability information between edge routers. BGP directs packets between autonomous systems (AS) -- networks managed by a single enterprise or service provider.

### Most used Network OSI Layers and their Usages
 - the MAC address this packet should go to (“Layer 2”)
 - the source IP and destination IP (“Layer 3”)
 - the port and other TCP/UDP information (“Layer 4”)
 - the contents of your HTTP packet like GET / (“Layer 7”)
 
### Types of Encapsulations
- IP-in-IP
ip-in-ip encapsulation just slaps on an extra IP header on top of your old IP header. 
```sh
MAC:  11:11:11:11:11:11
IP: 172.9.9.9
IP: 10.4.4.4
TCP stuff
HTTP stuff
```

- vxLan
vxlan encapsulation takes your whole packet (including the MAC address) and wraps it inside a UDP packet.
```sh
MAC address: 11:11:11:11:11:11
IP: 172.9.9.9
UDP port 8472 (the "vxlan port")
MAC address: ab:cd:ef:12:34:56
IP: 10.4.4.4
TCP port 80
HTTP stuff
```


### Some Table rerouting demo
What if I had a packet for the container 10.4.4.4 but I actually wanted it to go to the computer 172.23.1.1?
Change Route Table
```sh
sudo ip route add 10.4.4.0/24 via 172.23.1.1 dev eth0

```

#### Set up a new network interface with encapsulation configured.
[Tunneling Basics](http://www.linux-admins.net/2010/09/tunneling-ipip-and-gre-encapsulation.html)
```sh
sudo ip tunnel add mytun mode ipip remote 172.9.9.9 local 10.4.4.4 ttl 255
sudo ifconfig mytun 10.42.1.1
```

#### Then set up a route table, but tell Linux to route the packet with your new magical encapsulation network interface.

```sh
sudo route add -net 10.42.2.0/24 dev mytun
sudo route list 
```

