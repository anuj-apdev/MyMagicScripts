# IPTables is a saviour
```sh
iptables -vL -t filter
iptables -vL -t nat
iptables -vL -t mangle
iptables -vL -t raw
iptables -vL -t security
```

# Route commands (Should be replaced by ip -r)
```sh
route
netstat -tlpn
ip route list
```

# SHow cgroups
```sh
cat /proc/cgroups
```

