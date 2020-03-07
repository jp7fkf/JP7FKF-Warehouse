# CCNP - Route

## RIP
- RIPv1, RIPv2
  - UDP: 520

## RIPng(IPv6)
- UDP: 521

## Distributed List
- interfaceから出す/入るprefixをフィルタできる．
- refistributeと併用することによりredistributeされるprefixを制限することができる．
```
(config)# router <protocol>
(config-router)# distribute-list <acl-number> <in|out> [interface-id|source-protocol]

# もしくは prefix-listを使って(ip prefix-list <prefix_list_name> permit <prefix:xxx.xxx.xxx.xxx/xx>)
(config-router)# distribute-list prefix <prefix_list_name> <in|out> [interface-id|source-protocol]
```

- OSPFの場合インタフェースにoutを適用しても動作しない．
  - 再配布する場合はrouting table上は通常通り動作する．
  - LSA自体のフィルタリングはされないため，LSDBは構成される．

## IP Prefix List
- refistribute-listではACLとprefix-listの利用が選べるが，prefix-listを用いた方が高速処理される利点がある．
- `ip prefix-list <prefix_list_name> permit <prefix:xxx.xxx.xxx.xxx/xx>)`

### OSPFのエリア間(ABR)における制限
```
(config)# router ospf <process-id>
(config-router)# area <area-id> filter-list prefix <name> [ in | out ]
```

## route-map
- 暗黙のdenyつき．
```
(config)# route-map <map-tag> <permit|deny> [seq-number]
(config-route-map)# match <condition>
(config-route-map)# set <action>
```

- match条件
  - `ip address <acl>`
  - `metch length <min> <max>` # L3パケット長
  - `ip address prefix-list <name>`
  - `interface <interface-id>`
  - `tag <tag_name>`
  - `metric <metric>`
  - `route-type <type>` # ex. local,internal,external,type1,type2,nssa-external
  - `ip next-hop <acl>`
  - `ip route-source <acl>`
  - `as-path <prefix_list_num>`
  - `community <list_num>`
  - `extcommunity <list_name>` #拡張コミュニティリスト
  - `local-preference <value>`

- set
  - `ip next-hop <address>` # routingにかかわらずnexthopを指定
  - `interface <interface_id>` # routingにかかわらず転送先IFを指定
  - `default next-hop <address>` # PBRでmatchするnexthopがないときのdefault nexthop
  - `ip precedence <value>` # IP headerのprecedenceを指定(ToS Fielt first 3bits)
  - `metric <value>`
  - `metric-type <value>`
  - `tag <tag>`
  - `as-path prepend ...`
  - `community ...`
  - `dampening ...`
  - `local-preference ...`
  - `origin ...`
  - `weight ...`

## PBR(Policy Base Routing)
- route-mapを着信interfaceで適用させる
- 着信したパケットはまずroute-mapに従って転送させれる．matchするconditionがなかった場合，routing-tableによって転送される．
- 一般に処理は通常のルーティングと比べて遅くなる．(software処理になる？多分今時はそうではなさそう．)
```
(config)# interface <interface-id>
(config-if)# ip policy route-map <map-tag>

# ローカルPBR(自身発のパケットに対してPBRする)
(config)#ip local policy route-map <map-tag>
```

## AD値変更
- 変更はローカルのみ．
- 1-255
- `address/mask` を指定するとroute-sourceのルータからの広告に対して適用する
- aclを指定するとpermitされたものにだけ適用する
```
(config)# router protocol
(config-router)# distance <ad-value> <address/mask> [acl]
```

## IP SLA
- サービスレベルをモニタする機能
- 有効化することで測定用のパケットを任意の宛先へ早出できる．
- 遅延, RTT,ジッタ等の測定，死活監視等をすることができる．
```
(config)# ip sla <number> # operation num
(config-ip-sla)# icmp-echo <destination-address> [ source-ip <address> | source-interface <interface-id>]
(config-ip-sla-echo)# timeout <seconds>
(config-ip-sla-echo)# frequency <seconds> #interval

# sla scheduring
(config)# ip sla schedule <number> life [ forever | seconds ] start-time [ now | time ] [ recurring ]
```

## object tracking
- Catスイッチの場合，IP ServicesのFuture setが必要
- Routerの場合SECURITY, UC, DATAのいずれかのライセンスが必要
- SLAに基づくパス制御等の実行
```
(config)# track <object-number> ip sla operation-number [ state | reachability ]
(config-track)# delay [ down seconds | up seconds ]
(config)# ip route <network> <mask> <next-hop> track <object-number> # ex. static routeの追加
```

## IPv6
- 128bits/16bytes

## [CCNA & CCNP Practice Tests  | Exam Questions | LearnCisco.net](https://www.learncisco.net/cisco-practice-tests.html)

## [Router Alley - Guides](http://www.routeralley.com/guides.html)

## VPN
- IPsec-VPN
- SSL-VPN
- IP-VPN(MPLS)

## Tunneling Protocols

### GRE
- Multicast通る．//つまりdynamic routingできる/しやすい．
- RFC(s): RFC1701, RFC1702, RFC2784
- IP Protocol Number: 47

- 4bytesのGRE HeaderとIP Header(20bytes)を付加する
  - L3MTUは24バイト小さくなる

- ポイント
  - RIBにdestのIPがあれば，Tunnel Interface は"UP"となる．
    - そのためIOSにはkeepaliveが実装された．

- config
```
(config)# interface tunnel number
(config-if)# ip address <int_address> <mask>        # intのIP．つまりトンネルのパスのセグメントのIP．GREトンネルの両端で同一セグになる．
(config-if)# tunnel source [src_address|interface]  # srcのトンネル終端IP．一般にGlobalになることが多い．
(config-if)# tunnel destination <desc_address>      # dstのトンネル終端IP．一般にGlobalになることが多い．
(config-if)# tunnel mode gre ip                     # tunnel intのmodeはdefaultでgreとなる

# (Optional) keepalive
(config-if)# keepalive <period> <retries>           # period(0-32767): Keepalive period(default:10sec), Retries(1-255): keepalive retries
```

### IPsec
  - (単体では)Multicast通らない．

## GRE over IPsec
- GRE over IPSecのconfig
1. ISAKMPポリシーの設定（IKEフェーズ1に関する設定）
2. IPsecトランスフォームセットの設定（IKEフェーズ2に関する設定）
3. 暗号ACLの設定（IPsecの対象となるトラフィックの定義）
4. IPsecポリシーの設定（暗号マップの設定）
5. GREトンネルの作成
6. インターフェースに対する暗号マップの適用

```
# ISAKMP policy(IKE phase1)
(config)#crypto isakmp policy <priority>

# set hash algorithms
(config-isakmp)#encryption [des|3des|aes|aes 192|aes 256]
(config-isakmp)#hash [sha | md5]

# authentication setting
(config-isakmp)#authentication ｛rsa-sig | rsa-encr | pre-share｝

# DH(Diffie-Hellman) group
(config-isakmp)#group ｛1 | 2 | 5｝

# set lifetime
(config-isakmp)#lifetime <sec>

# association: psk and IPaddrs
(config)#crypto isakmp key <psk> address <peers_ipaddr>

# IPsec trasform-set(IKE phase2)
(config)#crypto ipsec transform-set <name> <transform1> [transform2] ...

# IPsec mode
(cfg-crypto-trans)#mode [transport | tunnel]

# encription ACL
(config)#access-list <num> permit gre <source_ip> <wildcard_mask> <dest_ip> <wildcard_mask>

# IP sec policy(cript map)
(config)#crypto map <name> <seq_num> ipsec-isakmp

# IPSec peers IPaddress setting
(config-crypto-map)#set peer [hostname | ip_address]

# set transform-set
(config-crypto-map)#set transform-set <name>

# set cript traffic
(config-crypto-map)#match address <acl_name(num)>

# GRE tunnel
# set tunnel interface
(config)#interface Tunnel <interface_id>

# set ip addr
(config-if)#ip address <ip_address> <netmask>

# set src interface (real interface)
(config-if)#tunnel source <ip_address / interface>

# set dest addrs (real interface)
(config-if)#tunnel destination <dest_addr>

# apply cript map
(config-if)#crypto map <name>
```

### L2TP
  - L2通る．

### Keywords
- HMAC（ Keyed-Hashing for Message Authentication Code

## ACL
- 暗黙のdenyは常にある．表示されない．
- 標準ACLは宛先に近い場所に設定，拡張ACLは送信元に近い場所に設定するのが推奨
- シーケンス番号を指定して条件文を設定するためには名前付きACLの構文を利用する必要がある

- 標準ACL
  - 番号: 1 - 99，1300 - 1999
```
(config)# access-list <number> [ permit | deny ] <source> <wildcard>
(config)# interface <interface-id>
(config-if)# ip access-group <number> [ in | out ]

# 名前付き
(config)# ip access-list standard <name>
(config-std-nacl)# <number> [ permit | deny ] <source> <wildcard>

```
- 拡張ACL
  - 番号: 100 - 199，2000 - 2699
```
(config)# access-list <number> [ permit | deny ] [protocol] <source> <wildcard> [port] <dest> <wildcard> [port] [ established | log | log-input ]
(config)# interface <interface-id>
(config-if)# ip access-group <number> [ in | out ]

# 名前付き
(config)# ip access-list extended <name>
(config-ext-nacl)# <number> [ permit | deny ] [protocol] <source> <wildcard> [port] <dest> <wildcard> [port] [ established | log | log-input ]
(config)#access-list 100 permit tcp any any eq ?
  <0-65535>    Port number
  bgp          Border Gateway Protocol (179)
  chargen      Character generator (19)
  cmd          Remote commands (rcmd, 514)
  daytime      Daytime (13)
  discard      Discard (9)
  domain       Domain Name Service (53)
  echo         Echo (7)
  exec         Exec (rsh, 512)
  finger       Finger (79)
  ftp          File Transfer Protocol (21)
  ftp-data     FTP data connections (20)
  gopher       Gopher (70)
  hostname     NIC hostname server (101)
  ident        Ident Protocol (113)
  irc          Internet Relay Chat (194)
  klogin       Kerberos login (543)
  kshell       Kerberos shell (544)
  login        Login (rlogin, 513)
  lpd          Printer service (515)
  nntp         Network News Transport Protocol (119)
  pim-auto-rp  PIM Auto-RP (496)
  pop2         Post Office Protocol v2 (109)
  pop3         Post Office Protocol v3 (110)
  smtp         Simple Mail Transport Protocol (25)
  sunrpc       Sun Remote Procedure Call (111)
  syslog       Syslog (514)
  tacacs       TAC Access Control System (49)
  talk         Talk (517)
  telnet       Telnet (23)
  time         Time (37)
  uucp         Unix-to-Unix Copy Program (540)
  whois        Nicname (43)
  www          World Wide Web (HTTP, 80)
```

- status
  - 全てのACLのステータスを表示
    - `show access-lists`
      - matchしたpachet数がわかる
  - 特定の番号（名前）のACLのステータスを表示
    - `show access-lists [ number | name ]`
      - matchしたpachet数がわかる
  - 特定のプロトコルのACLのステータスを表示
    - `show protocol access-lists`

- VTY回線へのACLの適用
```
(config)# line vty 0 15
(config-line)# access-class [ number | name ] [ in | out ]
```

## To Stop Debugging
> To stop a debug, use the `no debug all` or `undebug all` commands. Verify that the debugs have been turned off using the command `show debug`.
> Remember that the commands no logging console and terminal no monitor only prevent the output from being output on the console, Aux or vty respectively. It does not stop the debugging and therefore uses up router resources.
- ref: [Important Information on Debug Commands - Cisco](https://www.cisco.com/c/en/us/support/docs/dial-access/integrated-services-digital-networks-isdn-channel-associated-signaling-cas/10374-debug.html)

## debug したときはterm monしないとみえないよ
- debug hogehogeしてもそのままじゃvtyターミナルに出てこない．
  - あたり前田のクラッカー
- offしたいときは `no term mon`

## syslog系
- severity
  - 0: emergencies
  - 1: alerts
  - 2: critical
  - 3: errors
  - 4: warnings
  - 5: notifications
  - 6: information
  - 7: debugging
- ciscoの場合`(config)# logging trap errors`でerror以上のseverityのlogをsyslog serverに送ることができるようになる．

- `(config)#logging buffered <buffer-size>`
  - defaultは4069bytes.
- `(config)#logging host <dest_hostname|dest_addr`
  - syslogを転送する．
- consoleに出力するログレベル
  -`(config)#logging console <level>`
  - level: 0～7の数字 or critical等のkeywords指定
  - `(config)# logging console debug` 相当がdefalutで有効になっているのでデフォでconsoleログが出る

- リモート端末への出力メッセージレベル変更(vty)
  - `(config)#logging monitor <level>`

- ログバッファへの出力メッセージレベル変更
  - `(config)#logging buffered <level>`

- Syslogサーバに送信するメッセージレベル変更
  - `(config)#logging trap <level>`
- `show logging`で現在の設定状態を確認することができる

- TODO: そういえばconsoleにさすとloginしなくてもlog出てきちゃうけどあれはlogin後だけにできるのだろうか

## PPPoE
- フェーズ
  - アクティブディスカバリフェーズ
    - PPPoEクライアントがPPPoEサーバを検索してセッションIDを取得し，PPPoEセッションを確立する
  - PPPセッションフェーズ
    - PPPオプションのネゴシエートと認証を行い，PPPセッションを確立する

- PPPoE client config example
```
(config)#interface Dialer <num> # logical interface
(config-if)#mtu 1492 # set mtu
(config-if)#ip address negotiated # set ip from ppp server
(config-if)#encapsulation ppp
(config-if)#dialer pool <bind-num/id> # bind with physical inteface
(config)#interface <num>
(config-if)#pppoe-client dial-pool-number <bind-num/id> # bind with logical interface
```
- PPPoE sever config example
```
bba-group pppoe <bba-name>
  virtual-template <num>
!
inteface FastEthernet0/0
  no ip address
  pppoe enable group <bba-name>
!
interface virtual-template1
  ip address <addr> <mask>
  peer default ip address pool <pool_name>
!
ip local pool <pool_name> <pool_start> <pool_end>
```
- `show pppoe session`
  - 現在のセッション状態を表示する

## AAAまわり
- AAAとは
  - Authentication
    - 認証
    - 正当なユーザであるかをなんかの手段で検証する
  - Authorization
    - 認可
    - 権限周りの話
    - 発行可能なコマンドやアクセス範囲を制限する
  - Accounting
    - ユーザ履歴
    - "何をしたか"を追えることだと思えば良さそう
    - 認証に成功したユーザに対して情報を収集する．
    - 課金とか調査とか使えるようにしておくこと．

- RADIUS(Remote Authentication Dial-In User Service)
  - IETF標準
  - UDP1812, UDP1813
    - 認証認可に1812, アカウンティングに1813
  - パスワードのみ暗号化

  - config
```
# radius server登録/obj create
(config)# radius server <radius_name>
(config-radius-server)# address ipv4 <address> auth-port <number> acct-port <number>
(config-radius-server)# key <string>

# 認証サーバグループへの登録
(config)# aaa new-model
(config)# aaa group server radius <group-name>
(config-sg-server)# server name <radius_name>

# Legacyな設定
(config)# radius-server host <address> [auth-port <port>] [acct-port <port>] [timeout <seconds>] [retransmit <retries>] key <string>
```

- TACACS+(Terminal Access Controller Access Control System)
  - TACACS自体はTACACSは米国国防省に端を発した技術で，RFC1492に規定．
  - TACACS+はTACACSのCisco独自拡張
  - TCP49
  - パケット全体を暗号化

- TACACSからの応答
  - ACCEPT: 認証許可
  - REJECT: 拒否
  - ERROR: 認証失敗
  - CONTINUE: 認証情報追加要求

  - config
```
# TACACS+サーバの登録
(config)# tacacs server <tacacs_name>
(config-server-tacacs)# address ipv4 <address>
(config-server-tacacs)# port <number>
(config-server-tacacs)# key <string>
(config-server-tacacs)# single-connection

# 認証サーバグループへの登録(複数のserver を1グループに登録可能)
(config)# aaa new-model
(config)# aaa group server tacacs+ <group-name>
(config-sg-server)# server name <tacacs_name>

# Legacyな設定
(config)# tacacs-server host <address> port <number> timeout <seconds> key <string> [single-connection]
```

- config
```
# 上までで各種方式のgroup登録自体は終わり．必要に応じて利用したいところにこのaaaを登録する
(config)# aaa authentication login default group <group-name>
(config)# aaa authentication dot1x default group <group-name>
(config)# aaa authorization network default group <group-name>
(config)# aaa authorization auth-proxy default group <group-name>
(config)# aaa accounting dot1x default start-stop group <group-name>
(config)# aaa accounting system default start-stop group <group-name>
```

- 802.1XではTACACS+, Kerberosは使用できず，RADIUSのみ利用可能

## Misc.
- [What is LAT and PAD services ? - 70842 - The Cisco Learning Network](https://learningnetwork.cisco.com/thread/70842)
  > DEC's Local Area Transport (LAT)
  > X.25 defines the relationship between a Date Terminal Equipment (DTE) node and a network's DCE. The CCITT also defines the attachment of asynchronous start-stop devices to the PSDN in recommendations X.3, X.28 and X.29. The asynchronous device is typically a low-cost terminal such as an IBM® 3151 or 3152. These terminals lack the intelligence of a full-function node on the network and rely on a device called a packet assembler/disassembler (PAD)

## Flexible Netflow
- components
  - flow record
  - flow exporter
  - flow monitor
  - flow sampler

- config
```
## flow recordv5
(config)# flow record <record_name>
(config-flow-record)# match <key_field>
(config-flow-record)# collect <non_key_field>

# show flow record

## flow exporter
(config)# flow exporter <name>
(config-flow-exporter)# source <interface>
(config-flow-exporter)# export-protocol <flow_version>
(config-flow-exporter)# destination <dest_ipaddr>
(config-flow-exporter)# transport udp <port_num>

# show flow exporter

## flow sampler
(config)# sampler <name>
(config-sampler)# mode random 1 out-of <rate>

# show sampler

## apply to interface
(config)# int ge 0/1
(config-if)# ip flow monitor <name> [sampler <sampler_name>] [input|output]
```

## Netflow
- versions
  - v1
    - 現在はほぼ使われていない
  - v5
    - 数多く利用されている@2020-02-19 22:06:06
  - v8
    - v5の集約キャッシュ対応版．フォーマットはv5と同一．
  - v9
    - テンプレートを定義．下位互換性なしだが，テンプレートの採用により新しいプロトコルへの対応可能．
  - v10(IPFIX)
    - v9をもとにしたもの．

- commands
  - `show ip cache flow`
  - `show ip flow export`

## DMVPN(Dynamic Multipoint VPN)
- cisco独自
- cef有効が必須
- 複数拠点のVPN設定を簡単に設定

- mGRE, NHRP, IPsecの技術の組み合わせ
- GRE over IPsecでのroutingが可能．

- NHRP(NextHop Resolution Protocol)
  - NHS(NextHop Server)から宛先へのネクストホップを取得．サーバ&クライアント方式．

- config
```
# Example: ハブアンドスポーク3拠点(nGRE, NHRP)
# Site Central
## mGRE
interface Tunnel0
ip address <Central_tunnelIP> <mask>
ip nhrp authentication <auth_str>
ip nhrp map multicast dynamic
ip nhrp network-id <net_id>
tunnel source <Central_src>
tunnel mode gre multipoint
tunnel key <key>

# Branch A
## mGRE
interface Tunnel0
ip address <BranchA_tunnelIP> <mask>
ip nhrp authentication <auth_str>
ip nhrp map <Central_tunnelIP> <Central_src>
ip nhrp map multicast <Centralのsrc>
ip nhrp network-id <net_id>
ip nhrp nhs <Central_tunnelIP>
tunnel source <BranchA_src>
tunnel mode gre multipoint
tunnel key <key>

# Branch B
interface Tunnel0
ip address <BranchB_tunnelIP> <mask>
ip nhrp authentication <auth_str>
ip nhrp map <Central_tunnelIP> <Central_src>
ip nhrp map multicast <Centralのsrc>
ip nhrp network-id <net_id>
ip nhrp nhs <Central_tunnelIP>
tunnel source <BranchA_src>
tunnel mode gre multipoint
tunnel key <key>
// Branch AとBranch Bはtunnel IP, tunnel source以外同一のconfigとなるはず．

# IPsecを有効にする(全ルータで実施)
## ISAKMP, IPsec trans set定義
crypto isakmp policy <policy_num>
encription aes 192                                                      // この辺りは適宜ポリシを設定
hash md5                                                                // この辺りは適宜ポリシを設定
authentication pre-shared                                               // PSK
group 5                                                                 // この辺りは適宜ポリシを設定(DHグループかな)
crypto isakmp key <key> address 0.0.0.0
crypto ipsec transform-set <transset_name> esp-aes 256 esp-sha512-hmac  // この辺りは適宜ポリシを設定
mode transport

## DMVPNトンネルへのIPsec Profileの適用
crypto ipsec profile <ipsec_profile_name>
set transform-set <transset_name>

interface Tunnel0
tunnel protection ipsec profile <ipsec_profile_name>

```
- `show dmvpn`
- ref
  - [Cisco Configuration Professional 1.0 ルーティングおよびセキュリティ ユーザーズ ガイド - サイト間 VPN [Cisco Configuration Professional] - Cisco](https://www.cisco.com/c/ja_jp/td/docs/nma/routingswitchingmgmt/configprofessional/ug/002/18185-01/18185-01-12.html#pgfId-195580)

## NTP
- config
```
# NTPサーバになる
(config)#ntp master [strunum]
# NTPクライアントになる
(config)#ntp server <address> [key <key_num>]
# peerする
(config)#ntp peer <address> [key <num>]

# 認証で使うkeyを設定する
(config)#ntp authenticate # NTP認証有効可
(config)#ntp authentication-key <key_num> md5 <string>
(config)#ntp trusted-key <key_num> # 鍵の有効化

# ACLを用いたアクセス制御
(config)#ntp access-group [peer|serve|serve-only|query-only] <standard_acl_num>
```

- `show ntp associations`
  - `*`: master(synced)
  - `#`: master(unsynced)
    - 4000sec以上のずれがある
  - `+`: selected
  - `-`: candidate
  - `~`: configured

## EVN(Easy Virtual Network)
- VRFと802.1q vlan tagを対応させてL3の仮想化/分離を実現
- 最大32個作成可能

- EVNで使えるルーティング
  - static
  - OSPFv2
  - EIGRP
  - multicast routing
    - PIM（PIM-SM，PIM-DM，PIM-SSM）
    - MSDP
    - IGMP

- EVNトランクで利用できないもの
  - ACL
  - NAT
  - NetFlow
  - IPv6
  - WCCP

- vnet global がデフォルトで定義(default routingtableに対応)
  - 全てのinterfaceはデフォルトでこのvnet globalに所属

- vnetごとのsubinterfaceが自動で作成される(vnet trunk interfaceのvnet tagがsubint_numになる)

- config
```
# vrf, vnet tag定義
(config)# vrf definition <vrf_name>
(config-vrf)# vnet tag <tag: 2～4094>
(config-vrf)# address-family [ipv4 | ipv6]

# edge interface setting
(config)# interface <interface_id>
(config-if)# vrf forwarding <vrf_name>
(config-if)# ip address <ip_address>

# vnet trunk interface setting
(config)# interface <interface_id>
(config-if)# vnet trunk
```
- ref
  - [Easy Virtual Network コンフィギュレーション ガイ ド，Cisco IOS XE Release 3S - Easy Virtual Network の概要 [Cisco IOS XE 3S] - Cisco](https://www.cisco.com/c/ja_jp/td/docs/cian/iosxe/iosxe3s/cg/006/evn-xe-3s/evn-overview-xe.html#91135)
  - [NW仮想化技術 <第3回> EVN](https://www.infraeye.com/2016/10/31/ccieb03/)

## VRF(Virtual Routing and Forwarding)
- VRFで動くrouting protocol #2020-02-20 02:37:11
  - RIP
  - OSPF
  - EIGRP
  - static

- RD値: `[0-65535]:[0-4294967295] `
  - 64bits(= 8bytes)

- config
```
# VRF定義
(config)# ip vrf <vrf_name>
(cofnig-vrf)# rd <route-distinguisher>

# interfaceをVRFにさす
(config)# interface <interface_id>
(config-if)# ip vrf forwarding <vrf_name>

# vrfでrouting protocolをうごかす(ospfの場合)
(config)# router ospf <process_id> vrf <vrf_name>
...

# vrfでrouting protocolをうごかす(eigrpの場合)
(config)# router eigrp <as_num>
(config-router)# address-family ipv4 vrf <vrf_name>
...

# vrfでstatic route
(config)# ip route vrf <vrf_name> ...
```
- `show ip vrf <vrf_name>`

## VRF-Lite
1.　VRFごとに独立したルーティングテーブルを保持
2.　VRFごとにルーティングプロトコルを設定することができる
3.　重複したアドレスを使用しても問題なく，トラフィックの完全な分離が可能
4.　MPLS機能はサポートせず，LDPやラベルスイッチングはサポートしない

- config
```
# example
(config)# ip vrf RED
(config-vrf)# rd 1:10

(config)# ip vrf BLUE
(config-vrf)# rd 1:20

interface GigabitEthernet0/0
ip address 10.0.0.1 255.255.255.0

interface GigabitEthernet0/0.10
encapsulation dot1Q 10
ip vrf forwarding RED
ip address 10.0.0.1 255.255.255.0

interface GigabitEthernet0/0.20
encapsulation dot1Q 20
ip vrf forwarding BLUE
ip address 10.0.0.1 255.255.255.0
```
- `show ip vrf`
- `show ip route vrf <hoge>`

- ref
  - [NW仮想化技術 <第1回> VRF・VRF-Lite](https://www.infraeye.com/2016/10/20/ccieb001/)
  - [NW仮想化技術 <第2回> VRF・VRF-Lite：Ciscoコンフィグ設定](https://www.infraeye.com/2016/10/25/ccieb02/)

## Misc.
- debugで大量にログが出る可能性があるときは
  - console にログを出さない．(`no logging console`)
    - console出力はpriorityが高いのでhungする可能性あり．
    - vtyに出す．(term mon)
  - CPU使用率が十分低いことを確認
    - 高いとふつうにhungする．

- UDP dominance
  - 帯域がUDPによって占有されてしまう現象
  - QoSとかいれてないと引き起こされる．
  - UDPにはフロー制御やウィンドウ制御がないために起こりうる．
  - これが起こるとTCPが落ちたりする．
  - TCPがバッファに乗らないことをTCP Starvation

- TCPグローバル同期
  - テールドロップ等の発生

  - 対策
    - RED(Random Early Detection)
      - キューが埋まる前に閾値を設けてテールドロップを防ぐ
    - WRED(Weighted RED)
      - REDの破棄率を優先度ごとに設定可能

## TCPまわり
- config
```
# windos-scaling
# 目安はBDP(Bandwidth delay product)
# BDP(KB) = 回線帯域(kbps) x delay(sec) / 8(bytes)
(config)# ip tcp window-size [0-1073741823]
# SACK: 届かなかったパケットのみを選択して再送できるようにする
(config)# ip tcp selective-ack
# add timestamp
# tcpヘッダにtimestampを埋め，RTTの計測精度をあげ，TCP再送タイマが最適化できる
(config)# ip tcp timestamp
# mss clamp
(config-if)# ip tcp adjust-mss [500-1460]
```

## CoPP(Control Plane Policing)
- CPUまで上がってくるトラフィックによるCPUリソースの枯渇を防ぐ的なやつ

- config
```
## Example
ip access-list extended ACL_TELNET
 10 permit tcp any any eq telnet
ip access-list extended ACL_ICMP
 10 permit icmp any any
!
class-map CMAP_TELNET
 match access-group name ACL_TELNET
class-map CMAP_ICMP
 match access-group name ACL_ICMP
!
policy-map PMAP_COPP
 class CMAP_TELNET
  police 8000
 class CMAP_ICMP
  police 8000
!
control-plane
 service-policy input PMAP_COPP
```
- [Control Plane Policing (CoPP)](http://changineer.info/ccie/cisco_security/control_plane_policing_copp.html)

## uRPF(Uniface Reverse Path Filtering)
- 利用する前提条件
  - cefが有効であること
- Mode
  - Loose
    - 受信したパケットの送信元IPアドレスがRIBに存在するかを見る．
    - 存在する場合は転送，存在しない場合は破棄．
  - Strict
    - 受信したパケットの送信元IPアドレスと受信したインターフェースの組み合わせがRIBに存在するかを見る．
    - 存在する場合は転送，存在しない場合は破棄．
  - VRF
    - 全ての機器でサポートされているわけではない．
    - VRFはインスタンスごとにRIBを持つため，受信したパケットに対応するVRFインスタンスごとにLoose Modeと同等の処理を行う．

- sequence
1. inbound ACL
2. uRPF
3. routing
4. outbound ACL

- config
```
# 有効化したいinterfaceにて
(config-if)#ip verify unicast source reachable-via [any|rx] [allow-default] [allow-self-ping] [ACL_num]
## any：Loose Mode
## rx ：Strict Mode
## allow-default: 受信したパケットの送信元アドレスの確認にデフォルトルートを含む
## allow-self-ping: 自身からのpingを許可
## ※uRPFが有効なインターフェースはデフォルトで自身のアドレスへのpingも破棄する．
## ACL: uRPFで破棄対象となったパケットを再チェックする．ACLでpermitであったパケットはuRPFで破棄対象となっても許可される．
```

## Memno
- configのしかた最終
- 覚えておくべき系．port num, mac num, dhcp-option, timer
- BGP同期
- uRPF config
- NTP broadcast
- [ネットワークでなぜ遅延が生じるのか](https://www.slideshare.net/junkato1272/ss-70431661)
- PPPoE, EVN, DMVPN, GRE, IPsec, Flow, IPv6, VRF, VRF-Lite, NAT, IP SLA, ISATAP,
- CoPP/uRPF/VRF/NATv6/NATv6/IPv6/DMVPN/GRE/EVN/DHCPv6/フレームリレー/IP SLA/Flexible NetFlow/PPPoE/TACACS+/UDPdominance/SNMP
- DHCPv6/NAT64/uRPF/DMVPN/ユニキャストフラッディング/EVN/VRF/NetFlow/セキュリティ/NTP/フレームリレー/再配送/PBR
