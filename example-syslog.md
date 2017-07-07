# Configure Device to log to a log-Server

Our Syslog Server has the ip of: `10.10.99.111`

We are logging everything ! (`level debug`) but you can set another level of log e.g. `level err`

```
configure
set system syslog global facility all level notice
set system syslog global facility protocols level debug
set system syslog host 10.10.99.111 facility all level debug

commit
save
exit
```
