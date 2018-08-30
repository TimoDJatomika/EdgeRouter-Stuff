# Point-to-Point Protocol over Ethernet

## example pppoe configuration
Plug the 'internet' into port eth0. This was tested with ppoe credentials from Deutsche Telekom.

```
set interfaces ethernet eth0 description WAN
set interfaces ethernet eth0 duplex auto
set interfaces ethernet eth0 ip
set interfaces ethernet eth0 pppoe 0 default-route auto
set interfaces ethernet eth0 pppoe 0 firewall in name WAN_IN
set interfaces ethernet eth0 pppoe 0 firewall local name WAN_LOCAL
set interfaces ethernet eth0 pppoe 0 mtu 1492
set interfaces ethernet eth0 pppoe 0 name-server auto
set interfaces ethernet eth0 pppoe 0 password 00000000
set interfaces ethernet eth0 pppoe 0 user-id 0000000000001111111111110001@t-online.de
set interfaces ethernet eth0 speed auto
```

## using a draytek vigor 130 as a "pure" modem
- https://www.draytek.de/einrichtung-internet-mit-t-online.html
- https://www.youtube.com/watch?v=6Whbg_KnumM

## configure NAT
If you are using nat and ppoe then you have to configure NAT on **eth0**. 

```
set service nat rule 5010 description 'Masquerade for WAN'
set service nat rule 5010 log disable
set service nat rule 5010 outbound-interface eth0
set service nat rule 5010 protocol all
set service nat rule 5010 source address 10.10.0.0/16
set service nat rule 5010 type masquerade
```
