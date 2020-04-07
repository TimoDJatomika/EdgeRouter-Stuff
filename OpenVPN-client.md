# OpenVPN config (Client)
This tutorial describes how to configure the EdgeRouter as a OpenVPN Client.

Usefull links:
- [Youtube: EdgeRouter OpenVPN to Private Internet Access!](https://www.youtube.com/watch?v=B9dXiKhDVl0) 
- [Youtube: Dedicated Private Internet VLAN and Wireless Network](https://www.youtube.com/watch?v=_TBj5MYmgQc)

## Basic setup
First you need to ssh into your EdgeRouter. Then create a directory where you store your OpenVPN files.

```
sudo su
mkdir -p /config/auth/example
```

In this example I have the following files:
- ca.crt (Root CA)
- client.key (User private key)
- client.crt (User certificate)
- openvpn-static-key-v1.key (for tls-auth)
- example.ovpn (OpenVPN client configuration (see below))

Make sure that `key.pem` has `chmod 600`

## Example of the OpenVPN config-file
This file could differ depending on your openvpn server setup.
```
client
dev tun 
proto udp
remote vpn.example.com
resolv-retry infinite
nobind
persist-key
persist-tun
key-direction 1
remote-cert-tls server
auth-nocache
auth SHA512
cipher AES-256-GCM

# files
ca /config/auth/example/ca.crt
cert /config/auth/example/client.crt
key /config/auth/example/key.pem
tls-auth /config/auth/example/openvpn-static-key-v1.key 1

```

## Setup the interface
If you already configured you EdgeRouter as a OpenVPN server then you need to change the network inteface from `vtun0` to something else (e.g. `vtun1`)

```
configure
set interfaces openvpn vtun0 description 'example vpn'
set interfaces openvpn vtun0 config-file /config/auth/example/example.ovpn
commit
save
```



## Setup an extra VLAN for clients
```
# create a new vlan (VLAN 10)
set interfaces switch switch0 vif 10 address 192.168.40.1/24
set interfaces switch switch0 vif 10 description 'example VLAN'
set interfaces switch switch0 vif 10 mtu 1500
```

## Setup a DHCP server
```
set service dhcp-server shared-network-name EXAMPLE-LAN authoritative disable
set service dhcp-server shared-network-name EXAMPLE-LAN subnet 192.168.40.0/24 default-router 192.168.40.1
set service dhcp-server shared-network-name EXAMPLE-LAN subnet 192.168.40.0/24 dns-server 1.1.1.1
set service dhcp-server shared-network-name EXAMPLE-LAN subnet 192.168.40.0/24 domain-name example.com
set service dhcp-server shared-network-name EXAMPLE-LAN subnet 192.168.40.0/24 lease 86400
set service dhcp-server shared-network-name EXAMPLE-LAN subnet 192.168.40.0/24 start 192.168.40.10 stop 192.168.40.100
```

## Setup NAT & routing
```
# setup NAT
set service nat rule 5020 description NAT-EXAMPLE-VPN
set service nat rule 5020 log disable
set service nat rule 5020 outbound-interface vtun0
set service nat rule 5020 source address 192.168.40.0/24 
set service nat rule 5020 type masquerade

# setup routing
set protocols static table 1 interface-route 0.0.0.0/0 next-hop-interface vtun0

set firewall modify VPN_EXAMPLE_ROUTE rule 10 description 'Subnet to VPN'
set firewall modify VPN_EXAMPLE_ROUTE rule 10 source address 192.168.40.0/24
set firewall modify VPN_EXAMPLE_ROUTE rule 10 modify table 1

# apply the firewall route to VLAN 10
set interfaces switch switch0 vif 10 firewall in modify VPN_EXAMPLE_ROUTE
```
