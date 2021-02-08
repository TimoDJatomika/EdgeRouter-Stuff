# Configure IPv6 Firewall
Unfortunately there is no option to configure IPv6 Firewall via the GUI

## Basic Firewall Options
This basic firewall allows users to ping a IPv6 device from the internet. All other traffic to the device is blocked (default-action drop). 

```
set firewall ipv6-name ipv6-fw default-action drop
set firewall ipv6-name ipv6-fw description 'IPv6 firewall'
set firewall ipv6-name ipv6-fw rule 1 action accept
set firewall ipv6-name ipv6-fw rule 1 log disable
set firewall ipv6-name ipv6-fw rule 1 protocol icmpv6
set firewall ipv6-name ipv6-fw rule 1 description 'allow ICMPv6 traffic'
set firewall ipv6-name ipv6-fw rule 10 action accept
set firewall ipv6-name ipv6-fw rule 10 state established enable
set firewall ipv6-name ipv6-fw rule 10 state related enable
```

## Allow one host to be publicly accessible
```
set firewall ipv6-name ipv6-fw rule 4 action accept
set firewall ipv6-name ipv6-fw rule 4 description 'allow access to host x'
set firewall ipv6-name ipv6-fw rule 4 destination address '2001:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx'
```
