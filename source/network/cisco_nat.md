# Cisco - NAT

## About NAT

### Static NAT
- 送信元が外部でも内部でも通信開始できる．
- `(config)# ip nat inside source static <local-ip> <global-ip>`
- `(config)# ip nat inside source static [tcp 192.168.0.1 22 100.1.1.1 220 extendable]` # ポートstaticに変換
    - local-ip: 内部local
    - global-ip: 内部global
- `(config-if)# ip nat <inside | outside>`
    - インタフェースが内部/外部であることを指定

### Dynamic NAT
- 下記の例の場合，送信元が内部(access-list)に定義されたものだけが変換対象であるため内部/外部の両方から任意に通信開始はできない．
- `(config)# ip nat pool <name> <start-ip> <end-ip> [netmask mask | prefix-length length ]`
  - natプールを定義．外部global．
- `(config)# access-list <number> permit <source> <wildcard>`
  - ACLで変換対象の送信元IPを定義
- `(config)# ip nat inside source list <number> pool <name>`
  - natを定義
- `(config-if)# ip nat inside | outside`
  - インタフェースが内部/外部であることを指定

### PAT(Port Address Translation)
- `(config)# access-list <number> permit <source> <wildcard>`
- `(config)# ip nat inside source list <number> interface <interface> overload`
- `(config-if)# ip nat inside | outside`

もしくは

- `(config)# ip nat pool <name> <start-ip> <end-ip> [netmask mask | prefix-length length ]`
  - natプールを定義．外部global．
- `(config)# access-list <number> permit <source> <wildcard>`
- `(config)# ip nat inside source list <number> pool <name> overload`
- `(config-if)# ip nat inside | outside`

## configuration
```
protocols bgp 65001
  ...(snip)...
```

## commands
- `show ip nat translations`

- 動的に生成された全てのNAT変換エントリをクリア
  - `clear ip nat translations *`
- 特定のダイナミック内部変換エントリをクリア
  - `clear ip nat translations inside <global-ip> <local-ip>`
- 特定のダイナミック内部変換エントリ、外部変換エントリの両方をクリア
  - `clear ip nat translations inside <global-ip> <local-ip> outside <local-ip> <global-ip>`

- ダイナミックNATのタイムアウト値の変更
  - `(config)# ip nat translation [tcp-timeout|udp-timeout|dns-timeout|icmp-timeout] timeout <secs> [never]`

## debug/t-shoot

## References
