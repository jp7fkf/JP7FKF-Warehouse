# CCNP - Others

## L2 Techs
### FrameRelay
- X.25 WANからエラー訂正機能を取り除いたWAN．
- L2としてHDLCを拡張したLAPF (Link Access Procedure for Frame Relay)を利用
- 上位層protocolに依存しないのでL3はIPなどが利用可能
- L1としてEIA/TIA-232，EIA/TIA-449，V.35，X.21，EIA/TIA-530等のシリアル回線が利用可能

- 用語
  - DTE(Data Terminal Equipment): 顧客側のNW装置．
  - FRAD(Frame Relay Access Device): フレームリレーのフレームを構成する装置．
  - DSU(Degital Service Unit): DTEをフレームリレーの通信回線に接続し，信号同期や通信速度の制御を行う回線終端装置
  - DCE(Data Circuit-terminating Equipment): フレームリレーでは，クロック信号の提供とフレーム転送をする装置．キャリア網内に設置されるフレームリレースイッチなど．
  - フレームリレースイッチ: フレームリレー網内のキャリアの機器．フレームリレー交換機．
  - アクセス回線: ルータ（DTE）とフレームリレースイッチ（DCE）間の物理的な通信回線．
  - トランク: フレームリレー網内におけるフレームリレースイッチ間を接続する通信回線．
  - CIR(Circuit Information Rate): フレームリレー網内の転送速度．
  - FECN(Forward Explicit Congestion Nortification)
    - 輻輳を検出したフレームリレースイッチが宛先に輻輳を通知する信号
  - BECN(Forward Explicit Congestion Nortification)
    - 輻輳を検出したフレームリレースイッチが送信元に輻輳を通知する信号
  - PVC(Permanent Virtual Circuit): 相手先固定接続
  - SVC(Switched Virtual Circuit): 相手先選択接続

- VC(Virtual Circuit):物理的には1本である通信回線に，論理的には複数の通信回線であるとみなされた回線のこと．フレームリレーでは，PVCとSVCの2種類がある．※ 日本ではPVCしか使用できない．
- DLCI(Data Link Connection Identifier)データリンク接続識別子．VC(仮想回線)を識別するもので，フレームリレーヘッダの中にある情報．DLCI番号はキャリアにより割り当てられ，ルータとローカルのFRスイッチ間でのみ意味を持つ番号．したがって，接続先の拠点とDLCI番号が同じでも問題ない．このDLCI番号と宛先IPアドレスが関連付け（フレームリレーマップ）されているので送出されるパケットは使用するDLCI番号が分かる．

- Inverse ARP: ローカルのDLCI番号と宛先ルータのIPアドレスのフレームリレーマップを動的に生成するプロトコル．
- フレームリレーマップローカルのDLCI番号と宛先ルータのIPアドレスのframe-relay mapを静的に設定したマッピング情報．
  - ※ 一般的にマッピングは Inverse ARP を使用するのではなくこのフレームリレーマップを使用する．
- LMI(Local Management Interface): ユーザのルータとキャリアのFRスイッチ間のシグナリング．LMIでルータとFRスイッチ間のステータスを保持する．LMIタイプには，cisco，ansi，q933aの3つがある．

- LMI Status
  - Active: PVCが有効であり正常に通信できる状態
  - Inactive: ローカルのルータとFRスイッチ間の接続には問題ないが，接続先のルータとFRスイッチ間に問題が発生．
  - Deleted: ローカルのルータとFRスイッチ間の接続に問題が発生．PVCの設定がない，LMIを受信していないなど．

- LMI Options
  - VC Status Message
    - PVCの接続状態を通知．ブラックホールに送信するのを防ぐ．
  - Multicasting
    - フレームリレーでmulticast有効にするオプション
  - Global Addressing
    - DLCIをフレームリレー全体で管理．
  - Simple Flow Control
    - フロー制御(simple)を実施する．FECN/BECNに対応していないデバイスなどで有効化するとよい．

- config
```
(config)# int serial 0/0
# encap Type
(config-if)# encapsulation frame-relay [ cisco | ietf ]
# LMI Type
(config-if)# frame-relay lmi-type [ cisco | ansi | q933a ]
# Inverse ARP(Default: enable)
(config-if)# no frame-relay inverse-arp <protocol> <dlci>
# staticな frame relay mapのconfig (recommended)
(config-if)# frame-relay map <protocol> <address> <dlci(local DLCI)> [ broadcast ]
  # broadcast をいれることでVC上でbroadcast, multicastが通りルーティングプロトコルが動くようになる
```

### PPPoE
- Phase
  - アクティブデスカバリフェース
  - PPPセッションフェーズ

- Packets
  - PADI(PPPoE Active Discovery Initiation)
    - Broadcast
    - PPPoEサーバを探索
  - PADO(PPPoE Active Discovery Offer)
    - Unicast
  - PADR(PPPoE Active Discovery Request)
    - Unicast
  - PADS(PPPoE Active Discovery Session-confirmation)
    - Unicast
    - Session IDを通知
  - PADT(PPPoE Active Discovery Terminate)
    - Unicast
    - terminateしたいときに，したい方から投げるので，clientから投げることもあればserverから投げられることもある．

- config
```
(config)#interface Dialer 1
(config-if)#mtu 1492
(config-if)#ip address negotiated                     // PPPoEサーバからアドレスを取得
(config-if)#encapsulation ppp
(config-if)#dialer pool <pool_id>                     // bind with physical interface

(config)#interface ge 1/0
(config-if)#pppoe-client dial-pool-number <pool_id>   // bind with logical interface
```
- `show pppoe Dialer <num>`
- `show pppoe session`

## NAT

## NPTv6

## NAT64
- wellknown prefix: `64:FF9B::/96`
- IPv6 <--> IPv4の相互のnatのことを指す

## 6to4トンネリング
- IPv6パケットをIPv4でカプセル化するオーバーレイトンネル
- `2002::/16`
  - `2002:<ipv4_addr(32bits)>:<subnet_id(SLA_ID)(16bits)>::/64`

- IPv6 IGP(RIPng, OSPFv4など)は動かない．
- 外のIPv4はGlobal addrである必要がある．

- 少なくとも6to4境界ルータで6to4アドレスがサポートされている必要がある．

- ipv4ヘッダが頭につけられるだけのイメージ
- config
```
# 6to4 自動トンネル
(config)# interface tunnel <interface_id>
(config-if)# ipv6 enable
(config-if)# ipv6 address <6to4_add(2002::/16)>
(config-if)# tunnel source <ipv4_source>
(config-if)# tunnel mode ipv6ip 6to4
## destの設定がないが，destはIPv4の宛先アドレスから決まるので必要ない
(config)# ipv4 route 2002::/16 Tunnel <interface_id>
## これいれておけば6to4 destアドレス当てトラフィックはトンネル通して6to4するのでよい．

# ちなみに手動トンネル(これただのGRE)
(config)# interface tunnel <interface_id>
(config-if)# ipv6 enable
(config-if)# ipv6 address <addr>
(config-if)# tunnel source <ipv4_source>
(config-if)# tunnel destination <ipv4_dst>
(config-if)# tunnel mode ipv6ip
```

## ISATAP(Intra-Site Automatic Tunnel Addressing Protocol)
- IPv4ユーザがIPv6デュアルスタックルータとトンネルを貼ってトンネル内でIPv6通信をするアーキテクチャ
- ISATAPトンネルと呼ぶ
- ホストに割り当てられるIPv6 Addr: `<global_prefix(64bits)>:<ISATAP_Identifier(32bit), 0000:5EFE>:<IPv4 Address(32bits)`
  - global prefixはRAで通知
- config
```
# isatap終端(ルータ側), クライアントはこれらの情報からトンネルする
(config)# ipv6 unicast-routing
(config)# interface <interface_id>
(config-if)# ip address <ipv4> <mask>

(config)# interface tunnel <interface_id>
(config-if)# ipv6 enable
(config-if)# tunnel source [<ip_address>|<interface>]
(config-if)# tunnel mode ipv6ip isatap
(config-if)# ipv6 address <ipv6-prefix/prefix-length> [eui-64]
(config-if)# no ipv6 nd suppress-ra
```

## 拡張ping
- 指定できる項目
  - protocol(default: ip)
    - appletalk，clns，ip，novell，apollo，vines，decnet，xns
  - Target IP
  - Repeat count(default: 5)
  - Datagram size(default: 100bytes)
  - Timeout(default: 2sec)
  - Source Address or Source Interface
  - ToS(Type of Service)(default: 0)
  - Don't Flag bit set/unset(defalut: no, false, unset)
  - Reply validation(default: no)
  - DataPattern(default: 0xABCD)
  - IP Header Options
    - Verbose
    - Record
    - Strict
    - Timestamp
  - Sweep Range of Sizes(Path MTU Discovery的な？)
    - Echo Sizeを変える
- example
```
Router A>enable
Router A#ping
Protocol [ip]:
Target IP address: 192.168.40.1
Repeat count [5]:
Datagram size [100]:
Timeout in seconds [2]:
Extended commands [n]: y
Source address or interface: 172.16.23.2
Type of service [0]:
Set DF bit in IP header? [no]:
Validate reply data? [no]:
Data pattern [0xABCD]:
Loose, Strict, Record, Timestamp, Verbose[none]:
Sweep range of sizes [n]:
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 162.108.21.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 36/97/132 ms
```
- [拡張 ping および拡張 traceroute コマンドの使用 - Cisco](https://www.cisco.com/c/ja_jp/support/docs/ip/routing-information-protocol-rip/13730-ext-ping-trace.html)

## 面白コマンド
- `(config)#username <username> autocommand show running-config`
- ICMP Destination Unreachable生成のrate-limit
  - `(config)#ip icmp rate-limit unreachable [<milli-sec>]`
    - milli-secに1回にレートリミットをかける．