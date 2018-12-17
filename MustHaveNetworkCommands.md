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

### Most used Network OSI Layers and their Usages
 - the MAC address this packet should go to (“Layer 2”)
 - the source IP and destination IP (“Layer 3”)
 - the port and other TCP/UDP information (“Layer 4”)
 - the contents of your HTTP packet like GET / (“Layer 7”)

