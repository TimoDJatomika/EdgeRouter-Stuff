
## Configuration EdgeOS
```
configure
set service nat rule 5000 description NAT-TO-WAN
set service nat rule 5000 log disable
set service nat rule 5000 outbound-interface eth0
set service nat rule 5000 protocol all
set service nat rule 5000 source address 172.22.1.0/24
set service nat rule 5000 type masquerade
commit && save && exit
```


## Configuration VyOS
```
configure
set nat source rule 100 description 'NAT-TO-ETH0'
set nat source rule 100 outbound-interface 'eth0'
set nat source rule 100 source address '192.168.99.0/24'
set nat source rule 100 translation address 'masquerade'
commit && save && exit
```
