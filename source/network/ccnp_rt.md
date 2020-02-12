# CCNP - Router

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
f
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

### Tunneling Protocols
- IPsec
  - Multicast通らない．
- GRE
  - Multicast通る．
- L2TP
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
To stop a debug, use the `no debug all` or `undebug all` commands. Verify that the debugs have been turned off using the command `show debug`.
Remember that the commands no logging console and terminal no monitor only prevent the output from being output on the console, Aux or vty respectively. It does not stop the debugging and therefore uses up router resources.
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
  - TDP49
  - パケット全体を暗号化

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

## DHCP Options
- 82
