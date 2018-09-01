# Port Forwarding / Destination NAT

## Helpful links
- https://help.ubnt.com/hc/en-us/articles/205231700-EdgeRouter-Destination-NAT
- https://help.ubnt.com/hc/en-us/articles/217367937-EdgeRouter-Port-Forwarding
- https://www.youtube.com/watch?v=7QSRNwFo6os

## intro
Port Forwarding and destination NAT are the same thing. 
In this simple tutorial we are manually configuring D-NAT meaning that we manually set the firewall rules and configure D-NAT and **not** using Auto-Firewall or *port-forwarding* (which is the easy way).

The advantage of manually configuring D-NAT is that you have more control about who can access port from the WAN.

We are assuming that you have a public (static) IP on eth0 (here we using 1.2.3.4 as the WAN address) and a device on your LAN with a private IP (e.g. 172.22.1.10). This tutorial demonstrates show to *port forward* the port 8443 on a device with the IP 172.22.1.10 on the WAN IP 1.2.3.4 on port 8443. 
You could also configure another port on your WAN interface.

### set firewall: everyone can access port 8443 from WAN
```
set firewall name WAN_IN rule 20 action accept
set firewall name WAN_IN rule 20 description 'Allow 8443'
set firewall name WAN_IN rule 20 destination port 8443
set firewall name WAN_IN rule 20 log disable
set firewall name WAN_IN rule 20 protocol tcp
```

### set firewall: only devices from the IP 2.3.4.5 can access all ports that are available
```
set firewall name WAN_IN rule 20 action accept
set firewall name WAN_IN rule 20 description 'Allow user with specific IP on WAN_IN'
set firewall name WAN_IN rule 20 log disable
set firewall name WAN_IN rule 20 protocol all
set firewall name WAN_IN rule 20 source address 2.3.4.5
```

### configure D-NAT

```
set service nat rule 1 description 'some description'
set service nat rule 1 destination address 1.2.3.4 # this is your WAN IP
set service nat rule 1 destination group
set service nat rule 1 destination port 8443
set service nat rule 1 inbound-interface eth0 # the WAN Interface
set service nat rule 1 inside-address address 172.22.1.10 # the internal device on your LAN
set service nat rule 1 inside-address port 8443 # the port from the internal device that you want to forward
set service nat rule 1 log enable # log every request
set service nat rule 1 protocol tcp
set service nat rule 1 type destination
```
