# edgerouter-openvpn-2.4.5-static
A statically linked openvpnd executable for use in Ubiquity EdgeRouter devices to support min-tls-version

## OpenVPN is way behind on EdgeRouter firmware

As of firmware 1.10.1 (4/14/2018), the version of OpenVPN in the Ubiquity firmware is 2.3.2. The current stable version is 2.4.5. I went ahead and compiled and statically linked OpenVPN latest stable as of 4/2018, which is 2.4.5. This wasn't just for fun, rather it was to support `tls-version-min 1.2`. Truely for the paranoid or the curious. Use it if you want, or don't.

## MAJOR CAVEAT

This executable does NOT support server mode. It is meant for client only usage !!

## Example ovpn file

```client
dev-type tun
proto udp
remote 1.2.3.4 443
resolv-retry infinite
nobind
persist-key
persist-tun
persist-remote-ip
ca /config/auth/vpn.pem
tls-client
remote-cert-tls server
auth-user-pass /config/auth/vpn.creds
reneg-sec 0
link-mtu 1570
cipher AES-256-CBC
auth SHA256
#tls-cipher TLS-DHE-RSA-WITH-AES-256-CBC-SHA
keysize 256
persist-key
persist-tun
ping 5
ping-restart 15
up /config/scripts/vpn.up
status /var/run/openvpn-vtun0.status
script-security 2
comp-lzo
mlock
fast-io
tls-version-min 1.2
```

## Note on lzo/lz4

I also went ahead and linked in lz4 support in case your provider supports that

## How to actually use this?

Put the following in a startup script in /config, which is persistent. Remember that overwriting anything other than those under /config will disappear on reboot! Use an init script the `mount -o bind` to get the job done here

```
# mkdir /config/bin && cp ~ubnt/openvpn /config/bin/openvpn
# mount -o bind /config/bin/openvpn /usr/sbin/openvpn
```
