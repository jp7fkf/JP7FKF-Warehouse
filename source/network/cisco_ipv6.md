# Cisco - IPv6

## About Cisco IPv6
- IPv6の有効化
  - `(config)# ipv6 unicast-routing`

- RA 送信インターバル
  - `(config-if)# ipv6 nd ra interval <maximum-secs> [minimum-secs] | msec maximum-ms [minimum-ms]`

- RA抑制
  - `(config-if)# ipv6 nd ra suppress [all]`

- IPv6 static route
  - `(config)# ipv6 route prefix/length [nexthop | interface[ distance]`

- RIPng

- OSPFv3

- BGP

## configuration
```
protocols bgp 65001
  ...(snip)...
```

## commands
- `show ip route`

## debug/t-shoot

## References
