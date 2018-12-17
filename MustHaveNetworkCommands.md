## Table Of Content
- [IPTables is a saviour](#iptables-is-a-saviour)
- [Route commands (Should be replaced by ip -r)](#route-commands--should-be-replaced-by-ip--r-)
- [Show cgroups](#show-cgroups)

#### IPTables is a saviour
```sh
iptables -vL -t filter
iptables -vL -t nat
iptables -vL -t mangle
iptables -vL -t raw
iptables -vL -t security
```

#### Route commands (Should be replaced by ip -r)
```sh
route
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
