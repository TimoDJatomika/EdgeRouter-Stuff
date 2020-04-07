# OpenVPN config (Server)
This tutorial describes how to setup a OpenVPN server on a EdgeRouter.

## Create certificates
Here is a list with files that you need. You can use the Software XCA for that
- ca.crt (Root CA)
- server.crt (Server Certificate)
  - To prevent MITM Attacks make sure you set 
     - X509v3 Key Usage: Digital Signature, Key Encipherment
     - X509v3 Extended Key Usage: TLS Web Server Authentication
- server.key (Key File for the Server Certificate)
- dh.pem (Diffie–Hellman key exchange  key; Good is 2048 bit)
- revocation-list.crl (Optional; Certificate Revocation List)

After you create the files copy all of them into `/config/auth/`

For you client config: Make sure `remote-cert-tls server` is set.

## Basic OpenVPN configuration
```
configure
set interfaces openvpn vtun0
set interfaces openvpn vtun0 mode server
set interfaces openvpn vtun0 server name-server 1.1.1.1 # change to your prepered one
set interfaces openvpn vtun0 server domain-name example.com # change to your prefered one
# set your network
set interfaces openvpn vtun0 server push-route 192.168.178.0/24 
# set the ranche for the openvpn clients. Clients will receive a IP address from this subnet
set interfaces openvpn vtun0 server subnet 192.168.177.0/24
```

## Certificate setup
As described above. Make sure you private key has `chmod 600`.

```
set interfaces openvpn vtun0 tls ca-cert-file /config/auth/ca.crt
set interfaces openvpn vtun0 tls cert-file /config/auth/server.crt
set interfaces openvpn vtun0 tls dh-file /config/auth/dh2048.pem
set interfaces openvpn vtun0 tls key-file /config/auth/server.key
# optional: set revocation list
set interfaces openvpn vtun0 tls crl-file /config/auth/revocation-list.crl
```

## Configure logging
```
set interfaces openvpn vtun0 openvpn-option "--log /var/log/openvpn.log"
set interfaces openvpn vtun0 openvpn-option "--status /var/log/openvpn-status.log"
set interfaces openvpn vtun0 openvpn-option "--verb 7"
```

## Firewall configuration
Don't forget to set NAT for the openvpn clients

```
set firewall name XXX rule XX action accept
set firewall name XXX rule XX description 'Allow OpenVPN'
set firewall name XXX rule XX destination port 1194
set firewall name XXX rule XX log disable
set firewall name XXX rule XX protocol udp
```
