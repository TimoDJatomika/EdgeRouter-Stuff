# Setup Simple Network Management Protocol

## Setup SNMP v2

```
configure

set service snmp description 'manage via PRTG'
set service snmp community superSecretPassword authorization ro
set service snmp contact 'John Bauer'
set service snmp location Cologne

commit
save

```

## Setup SNMP v3
this is also possible

