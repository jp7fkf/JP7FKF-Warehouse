# nmap

## options
- `-e <interface>`: インタフェース指定

## scripts
- たくさんスクリプトがある．便利．
- [Nmapを検証してみました【NSE編】 – (n)](http://n.pentest.ninja/?p=32488)
- [NSEDoc Reference Portal](https://nmap.org/nsedoc/)

### dhcp-discover を投げる
- [broadcast-dhcp-discover NSE Script](https://nmap.org/nsedoc/scripts/broadcast-dhcp-discover.html)
- `sudo nmap --script broadcast-dhcp-discover`
- `nmap -sU -p 67 --script=dhcp-discover <target_ip> -e <interface>` とか．

- ex.
```
% sudo nmap --script broadcast-dhcp-discover
Password:
Starting Nmap 7.70 ( https://nmap.org ) at 2019-05-27 23:45 JST
Pre-scan script results:
| broadcast-dhcp-discover:
|   Response 1 of 1:
|     IP Offered: 172.16.0.29
|     DHCP Message Type: DHCPOFFER
|     Server Identifier: 172.16.0.1
|     IP Address Lease Time: 5m00s
|     Subnet Mask: 255.255.255.0
|     Router: 172.16.0.1
|     Domain Name Server: 172.16.0.1
|     Renewal Time Value: 2m30s
|_    Rebinding Time Value: 4m22s
WARNING: No targets were specified, so 0 hosts scanned.
Nmap done: 0 IP addresses (0 hosts up) scanned in 1.54 seconds
```