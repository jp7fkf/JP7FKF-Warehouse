# Cisco commands

## ios
- https://www.cisco.com/public/library/iosplanner/reldesignation.html

## username password の数字の意味
username 'hoge' password 0 'pass' -> この0の意味はなんなのか？
→0は暗号化してない．7はcisco独自の暗号化方式によってencryptされていることを示している．

## Cisco IOS XE, IOS XR: how to check optical power
- IOS XE example:
  - `show interfaces tenGigabitEthernet 1/1/4 transceiver`

- IOS XR example:
  - `show controllers tenGigE 0/0/2/1 phy`
  - `show controllers dwdm 0/0/2/1 optics`

### シーケンス番号指定でACLを削除する
```
(config)# ip access-list {extended | standard} (number | name}
(config- {ext | std} -acl)# no <sequence-number>
```

### よく書くACL
```
ip access-list extended firewall-in-v4
remark DNS
deny   udp any any eq domain
remark NTP
permit udp any eq ntp x.x.x.x 0.0.0.255 eq ntp
permit udp any eq ntp x.x.x.x 0.0.0.255 eq ntp
deny   udp any eq ntp any eq ntp
remark SNMP
deny   udp any any eq snmp
remark SSDP
deny   udp any any eq 1900
remark NetBIOS
deny   tcp any any eq msrpc
deny   tcp any any eq 137
deny   tcp any any eq 138
deny   tcp any any eq 139
deny   tcp any any eq 445
deny   udp any any eq 135
deny   udp any any eq netbios-ns
deny   udp any any eq netbios-dgm
deny   udp any any eq netbios-ss
deny   udp any any eq 445
remark Bogon
deny   ip 0.0.0.0 0.255.255.255 any
deny   ip 10.0.0.0 0.255.255.255 any
deny   ip 100.0.0.0 0.65.255.255 any
deny   ip 127.0.0.0 0.255.255.255 any
deny   ip 169.254.0.0 0.0.255.255 any
deny   ip 172.16.0.0 0.15.255.255 any
deny   ip 192.168.0.0 0.0.255.255 any
deny   ip 198.51.100.0 0.0.0.255 any
deny   ip 203.0.113.0 0.0.0.255 any
deny   ip 240.0.0.0 15.255.255.255 any
deny   ip host 255.255.255.255 any
remark MyPrefix
deny   ip x.x.x.x x.x.x.x x.x.x.x x.x.x.x
deny   ip x.x.x.x x.x.x.x x.x.x.x x.x.x.x
permit ip any any
```

### DNS サーバを動かす
```
ip dns server
ip host www.example.local 192.168.0.1
ip dns primary example.local soa dns01.example.local root.example.local 3600 900 86400 3600
```
  - こんな感じでレコードをつっこむ．

### ACLとかがどういう順序で動くか
- ref.) [CiscoにおけるNATの処理順序](https://www.infraexpert.com/study/nat7.htm)
- `ip nat inside -> ip nat outside`
  1. IPSecの場合は入力アクセスリストをチェック 
  1. 復号化： CET または IPSec 
  1. 入力アクセスリストをチェック 
  1. 入力レート制限をチェック
  1. 入力アカウンティング 
  1. Webキャッシュにリダイレクト
  1. ポリシールーティング
  1. ルーティング
  1. Inside から Outside への NAT 
  1. クリプト（暗号化用のマップのチェックとマーク） 
  1. 出力アクセスリストをチェック 
  1. CBACによる検査
  1. TCPインターセプト 
  1. 暗号化
  1. キューイング

- `ip nat outside -> ip nat inside`
  1. IPSecの場合は入力アクセスリストをチェック 
  1. 復号化： CET または IPSec
  1. 入力アクセスリストをチェック 
  1. 入力レート制限をチェック
  1. 入力アカウンティング
  1. Webキャッシュにリダイレクト
  1. Outside から Inside への NAT
  1. ポリシールーティング
  1. ルーティング
  1. クリプト（暗号化用のマップのチェックとマーク）
  1. 出力アクセス リストをチェック
  1. CBACによる検査
  1. TCPインターセプト 
  1. 暗号化
  1. キューイング

### ACLの番号を振り直したい
- `ip access-list resequence <acl_num> <start_seq_num> <interval_sequence_num>`

## Cisco interfaces
- BE: Bundle Ether
- Lo: loopback
- Nu0: Null0 interface
- Ti0:  tunnel-ip, GREとか終端するのかな．
- Mg0/RSP0/CPU0/x: MgmtEth, management, RSP(Router Switch Processor)とCPU，if-numが後ろにつく． `rack/slot/module/port`のcomventionで，この場合0rack, RSP0slot, CPU0module, 0portを意味する．
- Hu: HundredGigE
- Te: TenGigE
- Ge: GigabitE
- Fe: FastE

## よく使うcommand
### IOS
#### BGP系
- `show ip bgp neighbors <neighbor-address> advertised-routes`

### Common
- startup-configとruning-configのdiffみたいとき．
  - - `show archive config differences`

- ciscoでtraceroute をとめる
  - `Ctrl-Shift-6`

## Firmware Update
-  cisco sw firm up
```
- show env: show hardware, show ver
- download firmware
- copy firm to device
- md5 check: veryfy /md5 <file_path>
- show boot system
- conf t: boot system <file_path>
- write mem
- reload
- show ver
```

- cisco ap CAPWAP -> autohnomous mode AIR-CAP1702I-Q-K9
```
- get ios image
- copy ios image to ap
- login via console
- `debug capwap console cli`
- `debug capwap client no-reload`
- `archive download-sw /create-space /overwrite tftp:<path_to_file>`
- dir flashとかshow flash, show bootして確認．
- `reload`
```
- ref: [Upgrade Firmware on a Switch through the Command Line Interface (CLI) - Cisco](https://www.cisco.com/c/en/us/support/docs/smb/switches/cisco-550x-series-stackable-managed-switches/smb5566-upgrade-firmware-on-a-switch-through-the-command-line-interf.html)
- ref: [7 Steps to Upgrade IOS Image on Cisco Catalyst Switch or Router](https://www.thegeekstuff.com/2011/06/upgrade-cisco-ios-image/)

## cisco のprivilege level
- 0-15の16段階．
- 0: user mode
- 15: privileged mode(enable/conf t)
- custom privilege::
```
privilege configure level 15 ip dhcp pool
privilege configure level 15 ip dhcp
privilege configure level 15 ip
privilege exec level 1 show running-config ip dhcp pool
privilege exec level 1 show running-config ip dhcp
privilege exec level 1 show running-config ip
privilege exec level 15 show running-config
privilege exec level 1 show
```
的な感じにprivilege levelを指定することができる．
`username <username> privilege <priv_level_num> password <password>`
のprivilegeと紐づく．当たり前だけど．

## cisco wlc snmp monitoring tips
- https://gist.github.com/tajibot/a5456f8187ca2c8c3328
- https://github.com/B4ckF0rw4rd/Zabbix-Templates/blob/Zabbix3/Template-Cisco-WLC-Discovery/Template%20Cisco%20WLC%20Discovery.xml

## VPLSのmacをみる．(IOS-XR)
- `show l2vpn forwarding bridge-domain VPLS:VLAN100 mac-address location 0/0/CPU0 | include c496`