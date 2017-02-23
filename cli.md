# 

In the background the device runs a VyOS. That means you can use the standard VyOs commands.

VyOs is based on Debian. If you type `sudo su` you get a `/bin/bash`

## Basic Stuff
confgirue -> brings you in the congiguration mode (you can compare that to *conf t* on cisco devices)
commit -> commits the changes 
save -> saves the chanes

Allways run commit and save if you want to save your configuration.

*exit* brings you back into "normal mode"

## show the running config
`show configuration`

### Basic Configuration - System
 Command | Function 
 --- | ---
 `configure` | go into configuration mode
 `set system host-name foobar` | set your router hostname to *foobar*
 `set system domain-name 8.8.8.8` | set the default DNS server
 `set system gateway-address 123.123.123` | set the default Gateway
 `set system ntp server 0.ubnt.pool.ntp.org` | set the ntp server (active per default). You can specify multiple NTP Server
 `set system time-zone Europe/Berlin` | set your timezone (use tab if you are in a different timezone)
 `set system login banner pre-login "This is a test \nAnd this is a new line\n"` set a banner (simular to banner in the cisco world)
 
 the `delete` command can delete a configuration (simular to e.g. `no shutdown` in the cisco world). E.g.: `delete system domain-name`
 
### Basic Configuration - Service
 Command | Function 
 --- | ---
 
 
