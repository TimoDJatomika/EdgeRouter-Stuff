# L2TP Setup

If you have problem connection with your end device just restart the router.

## setup firewall
```
set firewall name WAN_LOCAL rule 80 action accept
set firewall name WAN_LOCAL rule 80 description 'Allow l2tp vpn'
set firewall name WAN_LOCAL rule 80 destination port 500
set firewall name WAN_LOCAL rule 80 log disable
set firewall name WAN_LOCAL rule 80 protocol udp
set firewall name WAN_LOCAL rule 90 action accept
set firewall name WAN_LOCAL rule 90 description 'Allow l2tp vpn'
set firewall name WAN_LOCAL rule 90 destination port 4500
set firewall name WAN_LOCAL rule 90 log disable
set firewall name WAN_LOCAL rule 90 protocol udp
set firewall name WAN_LOCAL rule 100 action accept
set firewall name WAN_LOCAL rule 100 description 'Allow l2tp vpn'
set firewall name WAN_LOCAL rule 100 destination port 1701
set firewall name WAN_LOCAL rule 100 ipsec match-ipsec
set firewall name WAN_LOCAL rule 100 log disable
set firewall name WAN_LOCAL rule 100 protocol udp
set firewall name WAN_LOCAL rule 110 action accept
set firewall name WAN_LOCAL rule 110 description 'Allow l2tp vpn'
set firewall name WAN_LOCAL rule 110 log disable
set firewall name WAN_LOCAL rule 110 protocol esp
```

## setup ip sec
```bash
set vpn ipsec auto-firewall-nat-exclude disable
# also works if you don't specify the ipsec-interface (this is the wan interface)
# set vpn ipsec ipsec-interfaces interface eth0
set vpn ipsec nat-networks allowed-network 0.0.0.0/0
set vpn ipsec nat-traversal enable
```

## create user accounts
```bash
set vpn l2tp remote-access authentication local-users username john password supersecret
```

## setup ip range for remote users 
```bash
set vpn l2tp remote-access authentication mode local
set vpn l2tp remote-access client-ip-pool start 172.19.1.20
set vpn l2tp remote-access client-ip-pool stop 172.19.1.70
```

## setup PSK for remote users
```bash
set vpn l2tp remote-access ipsec-settings authentication mode pre-shared-secret
set vpn l2tp remote-access ipsec-settings authentication pre-shared-secret supersecretpreshareskey
set vpn l2tp remote-access ipsec-settings ike-lifetime 3600
```

## set public IP
```bash
set vpn l2tp remote-access outside-address <public-ip>
```

## set DNS Server for clients
```
set vpn l2tp remote-access dns-servers server-1 8.8.8.8
```

## don't forget to set NAT if you haven't already
```bash
set service nat rule 5003 description NAT-L2TP
set service nat rule 5003 log disable
# set correct outbound interface
set service nat rule 5003 outbound-interface eth0
set service nat rule 5003 protocol all
#  set correct source address ranche
set service nat rule 5003 source address 172.19.1.0/24
set service nat rule 5003 type masquerade
```

## troubleshooting 
If you have problems setting up L2TP consider to take a look at the following log files:
- /var/log/messages
- /var/log/charon.log
- note that the usernames aren't logged. 

I you want to see who is currently connected just type: "show interfaces"
