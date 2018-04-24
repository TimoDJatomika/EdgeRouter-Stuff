# 

In the background the device runs a VyOS. That means you can use the standard VyOs commands.

VyOs is based on Debian. If you type `sudo su` you get a `/bin/bash`

## Basic Stuff
`configure` -> go into the configuration mode (you can compare that to *conf t* on cisco devices)
`commit` -> commits the changes 
`save` -> saves the chanes

Allways run `commit` and `save` if you want to save your configuration.

*exit* brings you back into "normal mode"

## Show the running config
`show configuration` gives you an "human readable" version of the configuration. `show configuration commands` displays a configuration that you can use for later purpose e.g. copy and paste it to a new device to make a 1to1 backup. 

`show ip route` is similar to *Cisco IOS*: it gives you a list of all routes. 

### Basic Configuration - System
 Command | Function 
 --- | ---
 `configure` | go into configuration mode
 `set system host-name foobar` | set your router hostname to *foobar*
 `set system domain-name 8.8.8.8` | set the default DNS server
 `set system gateway-address 123.123.123` | set the default Gateway
 `set system ntp server 0.ubnt.pool.ntp.org` | set the ntp server (active per default). You can specify multiple NTP Server
 `set system time-zone Europe/Berlin` | set your timezone (use tab if you are in a different timezone)
 `set system login banner pre-login "This is a test \nAnd this is a new line\n"` | set a banner (simular to banner in the cisco world)
 
 the `delete` command can delete a configuration (simular to e.g. `no shutdown` in the cisco world). E.g.: `delete system domain-name`
 
### Basic Configuration - Service
 Command | Function 
 --- | ---
 `show dhcp leases` | show ip address, MAC adress, pool and client name
 

### Create a new System User
replace *<new-username>* with your username and *<superSecretPassword123>* with your **strong** password. The password is stored encrypted so this is the only time you see the password in plain text.

```
set system login user <new-username> authentication plaintext-password <superSecretPassword123>
set system login user <new-username> level admin
# optional: set full name
set system login user <new-username> full-name 'Firstname Lastname'

```
Now test if you can login with the new user and then delete the default user *ubnt*. This only works if the user isn't logged in (either via ssh or GUI).
```
delete system login user ubnt
```

### Configuration Switch
The Router can also act as a switch. Here is an example:
```
set interfaces switch switch0 address 172.22.1.1/24
set interfaces switch switch0 mtu 1500
set interfaces switch switch0 switch-port interface eth2
set interfaces switch switch0 switch-port interface eth3
set interfaces switch switch0 switch-port interface eth4
set interfaces switch switch0 switch-port vlan-aware disable
```

### Configuration DHCP Server
```
set service dhcp-server shared-network-name CLIENT-LAN authoritative disable
set service dhcp-server shared-network-name CLIENT-LAN subnet 172.22.1.0/24 default-router 172.22.1.1
set service dhcp-server shared-network-name CLIENT-LAN subnet 172.22.1.0/24 dns-server 8.8.8.8
set service dhcp-server shared-network-name CLIENT-LAN subnet 172.22.1.0/24 lease 86400
set service dhcp-server shared-network-name CLIENT-LAN subnet 172.22.1.0/24 start 172.22.1.10 stop 172.22.1.100
```

#### Configure static IP for device
```
set service dhcp-server shared-network-name MGMT-VLAN subnet 10.10.99.0/24 static-mapping cgn-monitor ip-address 10.10.99.11
set service dhcp-server shared-network-name MGMT-VLAN subnet 10.10.99.0/24 static-mapping cgn-monitor mac-address '52:54:xx:xx:xx:xx'
```


 
 
