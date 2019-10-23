# AWS Site-to-Site VPN
AWS has the option to create a Site-to-Site VPN tunnel from your AWS VPC to your home/office Network. This tutorial shows you how to configure your EdgeRouter to create a tunnel. 

Although there is the option to use BGP for routing we will just configure a static route.

## Prerequisite
You need to have a static IPv4 Address.

## AWS Site
Login to your AWS Console and click on VPC -> Virtual Private Network (VPN) -> Customer Gateways -> and create a new Customer Gateway. 

Next Virtual Private Gateway: Create a new Virtual Private Gateway for your VPC

After that click on Site-to-Site VPN Connections and create a new Site-to-Site connection. AWS will create two servers with two different IP addresses. It is recommended that you configure both tunnels.

Download the configuration file by clicking on *Download Configuration* and just select *Generic* as the Vendor.


## Configure your Router
First you need to configure the virtual interface. Change the IP Address according to configuration file that you downloaded in the previous step.

```
set interfaces vti vti0 address 169.254.41.130/30   # Inside IP Addresses -> Customer Gateway
set interfaces vti vti0 description 'AWS VPC FRA'   # just a name. 
set interfaces vti vti0 mtu 1436                    # specified in the configuration file 
```

Next set a static route. Change the subnet according to your AWS VPC Network.
```
set protocols static interface-route 10.20.0.0/16 next-hop-interface vti0
```

Now you can configure ipsec.
```
set vpn ipsec auto-firewall-nat-exclude enable
set vpn ipsec esp-group AWS compression disable
set vpn ipsec esp-group AWS lifetime 3600
set vpn ipsec esp-group AWS mode tunnel
set vpn ipsec esp-group AWS pfs enable
set vpn ipsec esp-group AWS proposal 1 encryption aes128
set vpn ipsec esp-group AWS proposal 1 hash sha1
set vpn ipsec ike-group AWS dead-peer-detection action restart
set vpn ipsec ike-group AWS dead-peer-detection interval 15
set vpn ipsec ike-group AWS dead-peer-detection timeout 30
set vpn ipsec ike-group AWS ikev2-reauth no
set vpn ipsec ike-group AWS key-exchange ikev1
set vpn ipsec ike-group AWS lifetime 28800
set vpn ipsec ike-group AWS proposal 1 dh-group 2
set vpn ipsec ike-group AWS proposal 1 encryption aes128
set vpn ipsec ike-group AWS proposal 1 hash sha1
# change to your WAN Interface
set vpn ipsec ipsec-interfaces interface eth6
set vpn ipsec nat-networks allowed-network 0.0.0.0/0
set vpn ipsec nat-traversal enable
```

You also need to to configure the site-to-site connection

```
# replace with your AWS VPN Server IP. You can find the public IP in the AWS config file. 
set vpn ipsec site-to-site peer <aws-server-ip> authentication mode pre-shared-secret
# set your AWS PSK here
set vpn ipsec site-to-site peer <aws-server-ip> authentication pre-shared-secret <your-psk>
set vpn ipsec site-to-site peer <aws-server-ip> connection-type initiate
set vpn ipsec site-to-site peer <aws-server-ip> description 'VPC tunnel 1'
set vpn ipsec site-to-site peer <aws-server-ip> ike-group AWS
set vpn ipsec site-to-site peer <aws-server-ip> ikev2-reauth inherit
# set your local WAN address
set vpn ipsec site-to-site peer <aws-server-ip> local-address <your-wan-ip>
set vpn ipsec site-to-site peer <aws-server-ip> vti bind vti0
set vpn ipsec site-to-site peer <aws-server-ip> vti esp-group AWS
```

Configure your firewall so that NAT is disabled for AWS traffic

```
set service nat rule 5011 description no-nat-aws
set service nat rule 5011 exclude
set service nat rule 5011 log disable
set service nat rule 5011 outbound-interface vti0
# replace with your aws subnet
set service nat rule 5011 source address 10.20.0.0/16
set service nat rule 5011 type masquerade
```
