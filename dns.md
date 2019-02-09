# DNS
The Edge Router can function as a little DNS Server. But keep in mind that this is only a basic DNS Server. You can only configure A Records. 

## enable DNS
Which Interface should be allowed to query the DNS Server ?

```bash
set service dns forwarding listen-on eth2.10
set service dns forwarding listen-on eth2.20
set service dns forwarding listen-on eth2.30
set service dns forwarding listen-on eth2
```

## add an entry
You just need the IP Address and the hostname. You have to give the "static-host-mapping" a name. In this case it's `cgn-qemu-01` but this has nothing to do with the DNS name. 
Just make sure that this is unique. 

```bash
set system static-host-mapping host-name cgn-qemu-01 alias cgn-qemu-01.example.com
set system static-host-mapping host-name cgn-qemu-01 inet 10.10.99.130
```

You can also have more then one Hostname pointing to one IP Address 

```bash
set system static-host-mapping host-name cgn-vm-win10 alias cgn-vm-win10.example.com
set system static-host-mapping host-name cgn-vm-win10 alias cgn-win-mgmt.example.com
set system static-host-mapping host-name cgn-vm-win10 alias prtg.example.com
set system static-host-mapping host-name cgn-vm-win10 inet 10.10.99.111

```
