# CCNP - Switch

## Port Security
- type
  - static
    - 手動でmacを入力
  - dynamic
    - フレーム受信時にmacを学習．mac address-tableにのみ追加．
  - sticky
    - フレーム受信時にmacを学習．mac address-tableとrunning-configに追加．
    - learnしてrun conに追加されたのを確認してsaveすればよい．

- violation mode
  - protect
    - 許可されていない端末からのフレームを破棄．
  - restrict
    - 許可されていない端末からのフレームを破棄．
    - SNMP/Syslogによる通知を行う．
  - shutdown
    - portをerrdisableとする．
    - SNMP/Syslogによる通知を行う．

- config
```
(config-if)# switchport port-security
# 許可MACアドレスの最大数の決定(defalut: 1)
(config-if)# switchport port-security maximum <number>
# violation-modeの設定．defalut はshutdown.
(config-if)# switchport port-security violation [ protect | restrict | shutdown]

# secure macの設定
# static
(config-if)# switchport port-security mac-address <address>

# sticky learning
(config-if)# switchport port-security mac-address sticky

# セキュアMACアドレスのエージングタイムの設定
(config-if)# switchport port-security aging time <minutes>

# aging type, absolute: 登録から時間経過後，通信途中であっても削除． inactivity: 最後の通信から指定時間経過したら削除
(config-if)# switchport port-security aging type [ absolute | inactivity]
```

## SDMテンプレート
- Switch Database Management
  - TCAMの機能の割り当てをテンプレートベースで調整することができる．
  - `(config)# sdm prefer { access | default | dual-ipv4-and-ipv6 | indirect-ipv4-and-ipv6-routing | routing | vlan }`
  - 設定変更は再起動後に有効となるので．設定反映にはwrite mem, reloadが必須．
-  `show sdm prefer`
  - 現在のsdmテンプレートの状態がみれる．
  - `show smd prefer routing`などで，仮にそれをpreferにした場合のリソース割り当て状況がみれる．
- `show platform tcam utilization`
  - 現在のシステムリソースの利用率がみれる．
- Catの機種によってはIPv6がデフォルトのtemplateでは利用できない場合がある．
  - `(config) # sdm prefer dual-ipv4-and-ipv6 default` 等を叩いてv4/v6テンプレートに変更する．

## PoE
- `(config-if)# power inline auto [ max milli-watts] | never | static [ max milli-watts]`
- `show power inline`

## LLDP
- multicast: 0180.C200.000E
- TLVベース
```
+----------+---------------------+------------------+
| TLV type |      TLV name       | Usage in LLDPPDU |
+----------+---------------------+------------------+
|     0    | End of LLDPDU       |     Mandatory    |
|     1    | Chassis ID          |     Mandatory    |
|     2    | Port ID             |     Mandatory    |
|     3    | Time To Live        |     Mandatory    |
|     4    | Port description    |     Optional     |
|     5    | System name         |     Optional     |
|     6    | System description  |     Optional     |
|     7    | System capabilities |     Optional     |
|     8    | Management address  |     Optional     |
+----------+---------------------+------------------+
```
- LLDPを送信しない
  - `(config-if) # no lldp transmit`
- LLDPを受信しない
  - `(config-if) # no lldp receive`

- 有効化するTLVタイプの指定example
  - `(config-if) # lldp med-tlv-select inventory-management`

- タイマー系
```
(config)# lldp holdtime 180 #(default: 120)
(config)# lldp reinit 3 #(default: 2)
(config)# lldp timer 60 #(default: 30)
```

- `show lldp`: LLDPがグローバルで有効化しているかの確認，各種タイマー値の確認
- `show lldp interface`: LLDPが有効化されているインターフェースに関する情報の確認
- `show lldp neighbors`: LLDPのネイバー情報の確認
- `show lldp neighbor detail`: LLDPのネイバー情報の詳細な確認
- `show lldp traffic`: LLDPトラフィックのカウンターの確認
- `show lldp errors`: LLDPトラフィックのカウンターエラーの確認
- `clear lldp counters`: LLDPのカウンターのクリア
- `clear lldp table`: LLDPネイバーテーブルの情報削除

- LLDP-MED: LLDP for Media Endpoint Devices

## CDP
- multicast: 0100.0CCC.CCCC

## Stack(StackWise/StackWise Plus/Flex-Stack)
- stackケーブルはループにする(推奨で必須ではない．帯域が減る．)
- stackを構成するにはIOSバージョン, License, SDMテンプレートの設定 の3つが一致している必要がある．
- StackWise
  - スタックのスループットが32Gbps
  - 送信元ストリッピングを使用する場合，送信元と宛先が同一SW内であってもスタックケーブルリングを巡回する．
- StackWise Plus
  - スタックのスループットが64Gbps
  - 送信元ストリッピングを使用する場合，送信元と宛先が同一SW内であってもスタックケーブルリングを巡回せずlocalで完結する．

- スタックマスター，スタックメンバから構成．マスターが制御をする
  - マスタ選出の優先順位は下記の通り．
  1. 現在スタックマスターであるスイッチ
  2. 最高のスタックメンバプライオリティを持つスイッチ
  3. デフォルトのインターフェイスレベルの設定を使用していないスイッチ
  4. 高いプライオリティ フィーチャ セットおよびソフトウェア イメージを組み合わせたスイッチ
    - ①. IP Service Feature/暗号化ソフトウェアイメージ
    - ②. IP Service Feature/非暗号化ソフトウェアイメージ
    - ③. IP Base Feature/暗号化ソフトウェアイメージ
    - ④. IP Base Feature/非暗号化ソフトウェアイメージ
  5. MAC アドレスが最小のスイッチ

- マスタ変化するとき
  - スイッチスタックがリセットされた
  - スタックマスターがスイッチスタックから削除された
  - スタックマスターがリセットされたか，電源が切れた
  - スタック マスターに障害が発生した
  - 電源の入ったスタンドアロンスイッチが追加されて，スイッチスタックメンバシップが増えた．
    - 既存のスタック構成に新しくスイッチを追加する場合，電源OFFの状態でスタックケーブルを接続してから電源をONにする．
    - そうしないとマスタが再度選出されてしまう．

- マスタがconfigをもち，メンバはマスタから定期的にコピーを受信する．

- congig
  - スタックメンバのプライオリティの設定
    - `(config)# switch <stackmember-number> priority <priority>`
  - スタックメンバ番号の再割り当て
    - `(config)# switch <current-number> renumber <new-number>`
  - `show switch`: スタックメンバ番号，役割，プライオリティ値，スタックMACアドレスの情報を表示．
  - `show switch neighbors`: スタックのネイバーを表示．
  - `show switch stack-ports summary`: スタックのケーブル長，スタックのリンク ステータス，ループバックステータスを表示．
  - `show switch detail`: スタックリングの詳細情報を表示．
  - `show platform stack manager all`: スタックプロトコルバージョンなど，すべてのスイッチスタック情報を表示．

## VSS(Virtual Switcing System)
- 構成要素
  - 仮想スイッチメンバ: いわゆるスイッチ筐体
  - VSL(Vistual Switch Link): 筐体間リンク

- MSS(Multi-Chassis EtherChannel)で上・下流とつなぐことがおおい．

## VLAN
- 802.1q
  - IEEE802.1Q
  - ethernetフレームの送信元MACアドレスとtypeの間に下記4bytesを挿入
    - TPID(Tag Protocol Identifier)
      - 16bits
      - IEEE802.1Qフレームであることを受信側に示す情報．0x8100 の値が入る．
    - PCP(Priority Code Point)
      - 3bits
      - IEEE802.1pで定義されたフレームの優先度を示す情報． 0～7 の値が入る．
    - CFI(Canonical Format Indicator)
      - 1bits
      - アドレス形式を示す情報．イーサネットの場合は 0 の値が入る．
    - VID(VLAN Identifier)
      - 12bits
      - VLAN IDを示す情報． 0～4095 の値が入る．
  - native vlan(default: 1)
    - CDP，PAgP，VTP，DTPなどのtrafficが流れる

- ISL(Inter-Switch Link)
  - EthernetフレームをISLでカプセル化するタイプ．
  - ヘッダどFCSが付加される．
  - native vlanはサポートされていない
  - 下記ヘッダとFCSあわせって30bytesが付加される．
```
+----------+------+---------------------------------------------------------------+
|  Field   | bits | description                                                   |
+----------+------+---------------------------------------------------------------+
| DA       |  40  | 宛先アドレス．「0x01-00-0c-00-00」 のマルチキャスト                |
| Type     |  4   | Ethernet「0000」　TokenRing「0001」　 FDDI「0010」　 ATM「0011」  |
| USER     |  4   | イーサネットの優先度，通常は「0」                                  |
| SA       |  48  | 送信側Catalystスイッチポートの48ビットの送信元MACアドレス            |
| LEN      |  16  | DA, Type, USER, SA, LEN, FCSを除く16ビットのフレーム長            |
| SNAP/LLC |  24  | 標準SNAP 802.2LLCヘッダ                                         |
| HSA      |  24  | SAの先頭の3バイト                                               |
| VLAN ID  |  15  | VLAN番号                                                       |
| BPDU/CDP |   1  | フレームがBPDUであるか，CDPであるかを示す情報                       |
| INDEX    |  16  | 診断用の送信元ポートのインデックス情報                              |
| RES      |  16  | FDDIなど追加の情報用に予約済みのフィールド                          |
+----------+------+---------------------------------------------------------------+
```
- ISLはオーバーヘッドが大きいので基本dot1qという感じ．

## switchport mode
- `(config-if)# switchport mode [ access | trunk | dynamic desirable | dynamic auto]`

- access
  - static: 管理者が手動でVLANを割り当てる
  - dynamic: 接続するデバイスによってポートが動的にVLANを変更できる．決定方式は大きく3種類．
    - MAC addressベース
    - サブネットベース
    - ユーザベースVLAN
      - 802.1Xで認証をかけてVLAN割り当てたりもできる．
- trunk
  - encapsulation dot1q
  - encapsulation isl
- dynamic disiable
  - DTPを送信し，対向ポートとネゴシエーション．対向ポートがtrunk, dynamic desiable, dynamic autoの場合trunkとなる．
- dynamic auto
  - DTPを送信し，対向ポートとネゴシエーション．対向ポートがtrunk, dynamic desiableの場合trunkとなる．

- DTP(Dynamic Trunking Protocol)
  - トランクのするかどうかを動的に決める
  - 対抗のswとネゴる．
  - Cisco独自
  - DTP送信の停止: `(config-if)# switchport nonegotiate`

## VTP
- 概要
  - VTPアドバタイズメントを送信してvlan情報の同期を取る
  - Cisco独自
  - 各SWは1つのVTPドメインのみに参加できる．
  - 同期の前提はVTPドメインが同じであること
  - versionは1, 2, 3がある．
    - version1と2では互換性がない．default VTP versionが異なる場合もあるので注意
    - version2ではトークンリングのサポート，認識不能TLVのサポート機能が付加
    - version3ではvlan-id 1-1005だけでなく拡張vlan(1006-4095)がサポート
- Message
  - 要約アドバタイズメント
    - VTPドメイン名，VTPパスワード，VLAN情報，リビジョンの4つが含まれる．
    - リビジョンはserverで設定変更するごとに1ずつ増加する
    - VTP advertisementはnative vlanポートで伝搬
    - デフォルトで300秒ごとに送信される
    - リビジョンが自分の持っている番号より大きい場合更新する
    - 新規追加スイッチのリビジョン番号の方が高いのにそのまま繋ぐと，そのリビジョンで上書きされるので事故る．
      - いったんtransparentにしてリビジョンを0にする(transparentモードは常にリビジョン0となるため)ことで回避できる．
  - サブセット アドバタイズメント
    - VLAN情報のリスト
  - アドバタイズメント要求
    - 自分より大きいリビジョン番号の要約アドバタイズメントを受信した時や，VTPドメイン名を変更した時，スイッチがリセットされた時に送信する
    - アドバタイズメント要求を受信したCatalystは，自身の要約アドバタイズメントとサブセット アドバタイズメントを送信する
  - VTP加入メッセージ
    - VTPプルーニングを有効にした際に，自身の持つVLANを伝え，そのVLANのトラフィックを受信する意思を隣接するCatalystに伝えるために利用

- Mode
  - Server
    - vlan作成，VTP送信，VTP転送，VTPによる同期が可能
  - Client
    - VTP転送，VTPによる同期が可能
  - Transparent
    - vlan作成，VTP転送が可能(VTPによる同期はできない)
  - Off, 最近はoffにもできるっぽい．

- VTP Pruning
  - トランクリンク上で不要なVLAN Trafficの伝播をしない機能
    - swのdownlinkにvlanが出てないのにtrunkに送信するのは無駄．そういうのを切る．
  - default vlan(0, 1, 1002-1005, 4095)はVTP Pruningの対象外
- config
```
# VTPバージョンの設定
(config)# vtp version [ 1 | 2 | 3]

# VTPドメイン名の設定
(config)# vtp domain <domain-name>

# VTPパスワードの設定(optional)
(config)# vtp password password

# VTPモードの設定(defalut: server)
(config)# vtp mode [ server | client | transparent]

# VTP pruning有効化
(config)# vtp pruning
```
  - `show vtp status`

## 音声vlan
- IP Phoneなどの音質が重要視されるものに対する機能
- IP PhoneとPCを異なるVlanに配置できる機能
- Cisco IP Phoneにはuplink(trunk)と別にPC接続用Portがあるものがある．
  - このPC用ポート(Access)のvlanとIP Phoneの音声のvlanを分けるということ
- CDPを用いて音声vlan IDを通知する

- config
  - access portに指定(事実上trunk+native vlanとなる?未確認)
    - `(config-if)# switchport mode access`
  - data vlanの指定(これがnativeになる．たぶん．)
    - `(config-if)# switchport access vlan <vlan-id>`
  - 音声vlan の指定(こっちがencapされる．たぶん．)
    - `(config-if)# switchport voice vlan <vlan-id>`

  - `switchport voice detect cisco-phone`なんてものもある．

- QoSの設定
```
(config)# mls qos
(config-if)# mls qos trust cos
など
```

## Private Vlan(PVLAN)
- Vlan IDは同じだが，相互に通信させない場合等に使える．

- vlan構成
  - Primary VLAN
    - 大元のvlan
  - Secondary VLAN
    - Primary VLANを内部で分割したVLAN. 隔離VLAN, コミュニティVLANがある．
- Port Mode
  - 混合ポート, P（Promiscuous）ポート
    - プライマリVLANに属するポート
    - 全てのポートと通信可
  - 隔離ポート, I（Isolated）ポート
    - セカンダリVLAN（隔離VLAN）に属するポート
    - 混合ポートとのみ通信可
  - コミュニティポート, C（Community）ポート
    - セカンダリVLAN（コミュニティポートVLAN）に属するポート
    - 混合ポート，および同じコミュニティVLAN内のポートと通信可

- config
  - Primary VLANの設定
```
(config)# vlan <vlan-id>
(config-vlan)# private-vlan [ primary | isolated | community]
(config-vlan)# private-vlan association <secondary_vlan_ids, ex.101-102>
```
  - 混合ポートの設定
```
(config)# interface <interface_id>
(config-if)# switchport mode private-vlan promiscuous
(config-if)# switchport private-vlan mapping <primary_vlan_id> <secondary_vlan_ids>
```
  - 隔離ポート，コミュニティポートの設定
```
(config)# interface <interface_id>
(config-if)# switchport mode private-vlan host
(config-if)# switchport private-vlan host-association <primary_vlan_id> <secondary_vlan_ids>
```
  - SVIへのmap
```
(config)# interface vlan <primary-vlanid>
(config-if)# private-vlan mapping <secondary_vlanlist>
```


## STP
- 言わずと知れたループフリー製造装置
- IEEE802.1D
- BPDU(Bridge Protocol Data Unit)を使って制御

- BPDU
  - Multicast: `0180.C200.0000`

  - BridgeID
    - Pridge Priority(2bytes)
      - 手動定義可能
    - MAC Address(6bytes)
    - 最も小さいものがルートブリッジ
  - パスコスト
    - 10G: 2
    - 1G: 4
    - 100M: 19
    - 10M: 100

- 動き
  - BPDU送信
  - ルートブリッジの選定(BridgeIDが最も小さいもの)
  - ルートポート(RP)の選出
    - ルートブリッジがパスコスト0のBPDUを送信．それに加算していく．
    - 各SWで選定．
    - 最もルートブリッジに近いポート
    - ルートブリッジまでのパスコスト(ルートパスコスト)が最も小さいポート
    - 同一だったらどうするんだ？(TODO:)
      - 送信元ブリッジIDが小さい方か
  - 指定ポート(DP)の選出
    - 各リンクで1ポート
    - 最も小さいルートパスコストを持つRPを持つ側のポート
    - 結局ルートブリッジは全ポートがDPとなる
    - 同一だったら送信元ブリッジIDが小さい方
  - 残りのポート(DPに指定されなかったポート)は非指定(NDP)ポートとなりブロッキング状態となる(BPDU受信のみ)

- ポート状態
  - ルートポート
    - Status: Forwarding
    - RP (Root Port)
    - 非ルートブリッジごとに１ポート選出される，ルートブリッジに最も近いポート．
    - 選定順位
      - ① 各ポートのルートパスコスト
      - ② 送信元ブリッジID
      - ③ 送信元ポートID
  - 指定ポート
    - Status: Forwarding
    - DP (Designated Port)
    - 各リンクごとに１ポート選出される，ルートブリッジに最も近いポート．
    - 選定順位
      - ① 各スイッチのRPのルートパスコスト
      - ② 送信元ブリッジID
      - ③ 送信元ポートID
  - 非指定ポート
    - Status: Blocking
    - NDP (Non Designated Port )
    - ルートポートと指定ポートに選出されなかったポート
- 送信元ポートIDはポートプライオリティ+ポート番号で構成

- Port status
  - ディセーブル: Disabled
    - 管理者によりポートがshutdownされている状態．何も転送されない
    - MAC Learning: ×
  - ブロッキング: Blocking
    - データフレームを転送せずに，BPDUの受信のみ行う状態
    - 全てのポートは最初はブロッキング状態から開始する．
    - MAC Learning: ×
  - リスニング: Listening
    - BPDUを送受信し合い，ルートブリッジ，ルートポート，指定ポートの選出を行っている状態．MACアドレスも学習せずデータも転送しない状態．
    - MAC Learning: ×
  - ラーニング: Learning
    - BPDUを送受信し合う．非指定ポートになれば即座にブロッキングへ．受信したフレームの送信元MACアドレスを学習しているがデータは転送しない．
    - MAC Learning: ○
  - フォワーディング: Forwarding
    - ポートが，最終的にルートポートまたは指定ポートになった状態．この状態になるとようやくそのポートで，ユーザのデータを転送するようになる．
    - MAC Learning: ○

- Port Status Sequence
  - Blocking(MAX Age: 20sec) -> Listening(Forward Delay: 15sec) -> Learning(Forward Delay: 15sec) -> Forwarding
    - Learning中にNDPに指定されたら即座にBlockingに遷移する

- コンバージェンス
  - ルートブリッジは2秒ごとにBPDUを送信．
  - NDPがあるSWではNDPから20秒BPDU受信しないと(MAX Age)STPの再計算を開始
    - Convergence Time: 50sec(20 + 15 + 15)
  - DPではLinkdown後に即座に再計算開始
    - Convergence Time: 30sec(15 + 15)

- 複数のvlanがあるSpanning Tree
  - CST(Common Spanning Tree)
    - 複数のvlanでも1トポロジを生成
  - PVST+
    - 複数のvlanでvlan eachにトポロジを生成
      - vlanごとにroot bridgeがいる．
    - `spanning-tree mode pvst`

- config
  - スパニングツリープロトコルの有効化
    - `(config)# spanning-tree vlan <vlan-id>`
  - ブリッジプライオリティの設定
    - `(config)# spanning-tree vlan <vlan-id> priority <priority>`

## RSTP
- IEEE802.1W
- 基本おなじSTAを採用
- RSTPでは非指定ポートは存在せずに代替ポートとバックアップポートがある．

## MST
- 大規模なSTPをしたいとき向け．
- 複数の同一トポロジvlanの計算を一度に実行できる．
- インスタンスを複数持つ
- ロングパスコスト(32bits/2bytes)を用いる．
- デフォルトコスト
  - Ethernet(10Mbps): 2,000,000
  - FastEthernet(100Mbps): 200,000
  - GigabitEthernet(1Gbps): 20,000
  - TemGigabitEthernet(10Gbps): 2,000
  - HundredGigabitEthernet(100Gbps): 200

## EtherChannel
- PAgP
- LACP

## HSRP(Hot Standby Router Protocol)
- Cisco独自のFirthop Redundancy Protocol
- multicast: 224.0.0.2, UDP(IP:17)：1985
- 仮想IPは物理IPとは異なるIP

- Priority
  - Default: 100
- Object Trackingによる減少値: 10
- Timer
  - Hello: 3sec
  - Hold: 10sec

- preempt
  - preemptを設定しておくと，自分のPriorityが最も高いことを確認したとき，Coup メッセージを送信し，Activeルータに速やかに切り替えることができる　

- Status
  - Init
    - Hello送信しない
    - 初期状態
  - Learn
    - Hello送信しない
    - hello受信していない状態．
    - 仮想IP判別がされていない
    - ActiveルータからのHello待ち
  - Listen
    - Hello送信しない
    - 仮想IPが認識されている
    - ActiveルータでもStandbyルータでもない
    - HoldTimeがタイムアウトしない限りActive/Standbyルータでないルータはこの状態でstay.
  - Speak
    - Hello送信する
    - Active/Standbyルータの選出に参加する
    - Active/Standbyに選出されるまでこの状態でStay
  - Standby
    - Hello送信する
    - グループ内で1台．
    - パケット転送はしない？
  - Active
    - Hello送信する
    - グループ内で1台．
    - 仮想IP(MAC)に送信されたパケットを転送する

- Version
  - Defalut: 1
  - `(config-if)# standby version [ 1 | 2]`
  - version1
    - group番号: 0-255
    - VMAC: 0000.0c07.acXX
    - Multicast: 244.0.0.2
  - version2
    - group番号: 0-4095
    - VMAC: 0000.0c9f.fXXX
    - Multicast: 244.0.0.102

- config
```
# HSRP有効化
(config-if)# standby <group_num(defalut: 0)> ip <vip>

# Priority変更(defalut: 100, 0-255)
(config-if)# tandby <group_num> priority <prio_num>

# Preemptの有効化(defauld: disable)
(config-if)# standby <group_num> preempt [delay [minimum <sec> | reload <sec> | sync <sec>]]
# delay timerも変更可能

# Timer変更 (defalut: hello 3sec, hold 10sec), holdはhelloの3倍以上にする
(config-if)# standby <group_num> timers <hello_sec> <hold_sec>

# Object Tracking
(config-if)# standby <group_num> track <interface_id> [decent_num(default: 10)]

(config)#track <obj_num> <tracking_target>
(config-if)# standby <group_num> track <obj_num> [decent_num(default: 10)]

# Plaintext Auth(default: disable)
(config-if)# standby <group_num> authentication text <string>

# MD5 Auth(default: disable)
(config-if)# standby <group_num> authentication md5 key-string <string>
```

## VRRP
- RFC
  - v2: RFC3768
  - v3: RFC5798
- IP Protocol: 112
- Multivast: 224.0.0.18
- 仮想IPは物理IPと同一にすることが可能
  - IP Address Ownerと呼ぶ

- defalut
  - virtual mac: `0000.5e00.01XX`
  - hello inverval: 1sec
  - hold timer: 3sec

- config
```
(config)# interface <interface-id>
(config-if)# vrrp <group-number> ip <vip>
(config-if)# vrrp <group-number> priority <priority(defalut: 100)>
(config-if)# vrrp <group-number> preempt

# hello invervalの変更
(config-if)# vrrp <group-number> timers advertise [msec] <seconds> # hold timerは自動でhelloの3倍になる
(config-if)# vrrp <group-number> timers learn # 現在のマスタルータのhelloを採用する

# preempt delay
(config-if)# vrrp <group-number> preempt delay [minimum <seconds>]

# Plaintext Auth(default: disable)
(config-if)# vrrp <group-number> authentication text <string>

# MD5 Auth(default: disable)
(config-if)# vrrp <group-number> authentication md5 key-string <string>
```

## GLBP(Gateway Load Balancing Protocol)
- シスコ独自
- IP/UDP: UDP Port: 3222
- HSRP, VRRPと異なり，1グループないで複数のデフォゲを持てる．
- デフォゲを冗長するプロトコル
- デフォゲ自体の負荷分散が可能
- 物理的に複数台のルータが1台に見える

- グループ内のルータでプライオリティが最も大きいルータ: AVG(Active Virtual Gateway)
- AVGによって仮想MACアドレスを割り当てられたルータ: AVF(Active Virtual Forwarder)
- 1グループで最大4つのAVFが動作可能

- params
  - hello interval: default 3sec
  - hold timer: default 10sec
  - preempt: enable, delay30sec

- 仮想mac: `0007.b400.XXYY`
  - XX: Group番号
  - YY: MACアドレス番号(AVFが最大4台なので01-04らしい．)
- ロードバランス方式
  - host-depended
    - ホストが使用するMACアドレスが常に同じになるように，ホストのMACアドレスに基づくロードバランス
  - round-robin
    - 仮想IPアドレスに対するARP応答として複数のAVFのMACアドレスが「順番」に返されるロードバランス
  - weighted
    - 　AVFがアドバタイズするウェイト値に基づくロードバランス

- config
```
(config)# interface <interface-id>
(config-if)# glbp <group-number> ip <vip>
# AVG Priority
(config-if)# glbp <group-number> priority <priority>
# preempt
(config-if)# glbp <group-number> preempt
# preempt delay
(config-if)# glbp <group-number> preempt delay [minimum <seconds>]
# timer
(config-if)# glbp <group-number> timers <hello-seconds> <hold-seconds>
# loadbalance config
(config-if)# glbp <group-number> load-balancing [ host-dependent | round-robin | weighted]
# Plaintext Auth(default: disable)
(config-if)# glbp <group_num> authentication text <string>

# MD5 Auth(default: disable)
(config-if)# glbp <group_num> authentication md5 key-string <string>
```

## DHCP
- UDP
  - client->sv: UDP67
  - sv->client: UDP68
- config
  - DHCP サーバ
```
# DHCPサービスの有効化
(config)# service dhcp
# 除外するIPアドレスの設定
(config)# ip dhcp excluded-address <ip-address | last-ip-address>
# DHCPプール名の設定
(config)# ip dhcp pool <pool-name>
# ネットワークアドレスの設定
(dhcp-config)# network <network> [<subnet-mask>|<prefix-length>]
# デフォルトゲートウェイの設定
(dhcp-config)# default-router <ip-address>
# DNSサーバの設定(optional)
(dhcp-config)# dns-server <ip-address>
# ドメイン名の設定(optional)
(dhcp-config)# domain-name <name>
# リース期間の設定(optional, defalut: 1day)
(dhcp-config)# lease [<days> <hours> <minutes> | infinite

# DHCPデータベースエージェントの設定(optional)
(config)# ip dhcp database <url> [timeout <seconds> | write-delay <seconds>]
```
  - DHCPリレーエージェント
```
# DHCPサービスの有効化
(config)# service dhcp
# 受信interfaceでunicast変換先IPを指定
(config-if)# ip helper-address <ip-address>

# ip-helperを指定するとデフォルトで下記ポートのパケットがユニキャスト変換される
+-----+-----+---------------------------------------+
| Port| Prot| description                           |
+-----+-----+---------------------------------------+
| 37  | UDP | time: Time                            |
| 42  | UDP | nameserver: Host Name Server          |
| 53  | UDP | domain: Domain Name Server            |
| 67  | UDP | bootps: Bootstrap Protocol Server     |
| 68  | UDP | bootpc: Bootstrap Protocol Client     |
| 69  | UDP | tftp: Trivial File Transfer           |
| 137 | UDP | netbios-ns: NETBIOS Name Service      |
| 138 | UDP | netbios-dgm: NETBIOS Datagram Service |
+-----+-----+---------------------------------------+

# 指定したprotocolだけ除外/追加可能
(config-if)# no ip forward-protocol udp tftp
(config)#ip forward-protocol udp ?
  <0-65535>      Port number
  biff           Biff (mail notification, comsat, 512)
  bootpc         Bootstrap Protocol (BOOTP) client (68)
  bootps         Bootstrap Protocol (BOOTP) server (67)
  discard        Discard (9)
  dnsix          DNSIX security protocol auditing (195)
  domain         Domain Name Service (DNS, 53)
  echo           Echo (7)
  isakmp         Internet Security Association and Key Management Protocol (500)
  mobile-ip      Mobile IP registration (434)
  nameserver     IEN116 name service (obsolete, 42)
  netbios-dgm    NetBios datagram service (138)
  netbios-ns     NetBios name service (137)
  netbios-ss     NetBios session service (139)
  non500-isakmp  Internet Security Association and Key Management Protocol (4500)
  ntp            Network Time Protocol (123)
  pim-auto-rp    PIM Auto-RP (496)
  rip            Routing Information Protocol (router, in.routed, 520)
  snmp           Simple Network Management Protocol (161)
  snmptrap       SNMP Traps (162)
  sunrpc         Sun Remote Procedure Call (111)
  syslog         System Logger (514)
  tacacs         TAC Access Control System (49)
  talk           Talk (517)
  tftp           Trivial File Transfer Protocol (69)
  time           Time (37)
  who            Who service (rwho, 513)
  xdmcp          X Display Manager Control Protocol (177)
```
- `show ip dhcp pool`
- `show ip dhcp conflict`
- `show ip dhcp binding`
- `debug ip dhcp server packet`

- Options
  - 43: WLCのIPアドレスを Cisco Aironet lightweight access points に伝達
  - 60: VCI（ Vendor Class Identifier：ベンダー クラス識別子 ）
    - ベンダー固有の製品型番など．テキスト．
  - 150: IP-Phoneの設定情報のダウンロードとなるTFTPサーバのIPアドレスをIP-Phoneに伝達
  - `(dhcp-config)# option code [ ascii string | hex string | ip address]`

## UDLD(UniDirectional Link Detection)
- 単一方向リンク検知
- 光ファイバ等片方向での障害が発生しやすい場合に効果的

- mode
  - aggressive
    - 光ファイバーリンクとツイストペアーリンク上の片方向トラフィックの検出が可能
    - err-disabledする
    - 光ファイバー接続におけるポートの誤った接続による単一方向リンクの検出が可能
  - normal
    - 光ファイバー接続におけるポートの誤った接続による単一方向リンクの検出が可能
    - 該当ポートをundeterminedにする．err-disabledしない

- 動作
  - Helloパケット(probe)を定期送信
  - 隣接UDLD機器はaging timer付きcacheを持つ
  - ネイバから再同期要求を受信するとEchoメッセージを送信する
  - このEchoが受信できなかった場合，障害状態とする．
  - エコーを受信できれば「Bidirectional」と見なす
    - detectionフェーズからadvertisementフェーズに移行する
    - advertisementフェーズではhelloを定期的(default: 15sec)に送出する

- config
```
# UDLDをグローバルに有効化
(config)# udld [aggressive | enable | message time <message-timer-interval>]
## enbaleはnormal mode
## message-timer-intervalはhello間隔．1-90sec, defalut:15sec

# UDLDをインターフェース上で有効化
(config)# interface <interface-id>
(config-if)# udld port [ aggressive]

# err-disableのreset
- shut-noshut
- 特権モードで #udld reset
- auto-recovery(errdisable recovery cause udld hoge...)
```

## Storm-Control
- ストコン．
- config
```
# ストーム制御としきい値レベルの設定
(config)# interface <interface-id>
(config-if)# storm-control | broadcast | multicast | unicast level level [ level-low] | bps bps [ bps-low] | pps pps [ pps-low]
- level: 上限閾値．帯域幅のpercentage指定(0.00-100.00)
- level-low: 下限閾値．帯域幅のpercentage指定(0.00-100.00)
 etc...

# ここでいうmulticast, unicastはいずれもunknownなものを指すらしい．実際そうあってほしいけど．

# action
(config)# interface interface-id
(config-if)# storm-control action [ shutdown | trap]
  - trapはsnmptrapする．
```

## 保護ポート
- 保護ポート同士のトラフィック転送をしない．
  - broadcast, multicastを含め．
  - PIM(Protocol Independent Multicast)はソフトウェア転送なので転送される

- 隔離vlan的なイメージ．単純版．

- config
```
# 保護ポートの有効化
(config)# interface <interface-id>
(config-if)# switchport protected
```

## ポートブロッキング
- unknown multicast, unknown unicastを送信しない設定に．
- config
```
# 未知のmulticast/unicastの転送(送信)をブロック
(config)# interface <interface-id>
(config-if)# switchport block unicast
(config-if)# switchport block multicast
```

## SPAN(Switched Port Analyzer)
```
(config)#monitor session ?
  <1-66>  SPAN session number

(config)#monitor session <session_num> source interface <interface-id> [both|tx|rx]
# vlan指定も可能
(config)#monitor session <session_num> source vlan <vlan-id> [both|tx|rx]

(config)#monitor session <session_num> destination interface <interface-id> [encapsulation replicate]
# encap. replicate: タグつけして送信
```

## RSPAN(Remote Switched Port Analyzer)
- mirrorしたpacketをtaggingしてvlanにのっけてやる
```
(config)# vlan <vlan-id>
(config-vlan)# remote-span

# おくるほう
(config)#monitor session <session_num> source interface <interface-id> [both|tx|rx]
(config)#monitor session <session_num> destination remote vlan <vlan-id>
# うけるほう
(config)#monitor session <session_num> source vlan remote vlan <vlan-id>
(config)#monitor session <session_num> destination interface <interface-id> [both|tx|rx]
```
- 標準VLAN（VLAN ID：1005以下）であれば，RSPAN VLANをVTPで伝搬可能
- 拡張VLAN（VLAN ID：1006 ～ 4094）をRSPAN VLANとして使用する場合は，送信元と宛先および通過するスイッチに手動で同じRSPAN VLANを設定する必要がある
- RSPANトラフィックが通過するすべてのスイッチでRSPAN VLANをサポートしている必要がある

## is 何
- 送信元ストリッピング
- PIMトラフィック

## err-disable
- shut-noshutで回復
- `(config)# errdisable recovery cause <hoge>`
    - 一定時間経過後recoveryとできる

## NSF(Non-Stop Forwarding)
- ネットワークレベルの冗長化機能
- SSO(Stateful Switch Over)と併用
- 冗長化したスーパバイザエンジンに障害した場合のスイッチオーバを素早く行いノンストップフォワーディングする

## ACL
- 評価順(基本下から上と思えばいい)
1. PACL(in)
1. VACL(in)
1. RACL(in)
1. RACL(out)

### RACL(Router ACL)
- いわゆるふつうのACL
- PortからSVIにあがってくるところにかける(SVIにかける)
- in or out
- config
```
(config)# interface gigabitethernet 0/1
(config-if)# no switchport
(config-if)# ip address 192.168.0.254 255.255.255.0
(config-if)# ip access-group 101 in

(config)# interface vlan 10
(config-if)# ip address 10.1.1.254 255.255.255.0
(config-if)# ip access-group 102 out

(config)# access-list 101 permit tcp any any eq www
(config)# access-list 102 permit tcp any host 10.1.1.1 eq www
```

### VACL(Vlan ACL)
- Portからvlan bridgeにあがってくるところにかける
- in only
- config
```
(config)# access-list <acl_num> [permit|deny] <ipaddr> <mask> # if use standard acl
(config)# vlan access-map <acm_name> <seq_num>
(config-access-map)# match ip address <acl_num>
(config-access-map)# action [drop | forward]
(config)# vlan filter <acm_name> vlan-list <vlan_id>

# example
(config)# ip access-list extended A-TCP
(config-ext-nacl)# permit tcp host 10.1.1.2 host 10.1.1.1
(config-ext-nacl)# permit tcp host 192.168.0.1 host 10.1.1.1

(config)# ip access-list extended A-IP
(config-ext-nacl)# permit ip any any
# MAC ACL
(config)# mac access-list extended <acl-name>
(config-ext-macl)# permit [ host <source-mac> | any] [ host <dest-mac> | any]

(config)# vlan access-map V-MAP 10
(config-access-map)# match ip address A-TCP
(config-access-map)# action drop

(config)# vlan access-map V-MAP 20
(config-access-map)# match ip address A-IP
(config-access-map)# action forward

(config)# vlan filter V-MAP vlan-list 10
```
- route-mapと似てるconfiguration.

### PACL(Port ACL)
- ポートにかけるACL
- in only

- config
```
(config)# ip access-list extended A-TCP1
(config-ext-nacl)# deny tcp host 192.168.0.1 host 10.1.1.2
(config-ext-nacl)# permit ip any any

(config)# ip access-list extended A-TCP2
(config-ext-nacl)# deny tcp host 10.1.1.2 host 10.1.1.1
(config-ext-nacl)# permit ip any any

# MAC ACL
(config)# mac access-list extended <acl-name>
(config-ext-macl)# permit [ host <source-mac> | any] [ host <dest-mac> | any]

(config)# interface gigabitethernet 0/1
(config-if)# no switchport
(config-if)# ip address 192.168.0.254 255.255.255.0
(config-if)# ip access-group A-TCP1 in

(config)# interface gigabitethernet 0/2
(config-if)# switchport mode access
(config-if)# switchport access vlan 10
(config-if)# ip access-group A-TCP2 in

# ポートモードの設定
(config-if)# access-group mode [ prefer port | merge]
  - prefer port: 優先ポートモード.PACLがL2ポートに適用している場合はPACLだけが有効になり，その他のACL（RACL VACL）を
　無効にする．trunk portではこちらだけが可能．
  - merge: マージモード(defalut).PACL，VACL，RACLの順番でフィルタリング.
```

## 802.1X
- IEEE802.1X認証を行うためにはサプリカント，認証装置，認証サーバの3つの構成要素が必要
  - サプリカントSupplicant)
    - IEEE802.1Xにおけるクライアントのこと．またはクライアントにインストールするソフトウェア．
    - 認証を受けるクライアントはPCにインストールする必要があるが，最近のPCには標準搭載されている．
  - 認証装置(Authenticator)
    - サプリカントと認証サーバの仲介役となるネットワーク機器．IEEE802.1X対応のLANスイッチまたは無線LANアクセスポイントのこと．
    - これらの機器はサプリカントと認証サーバとの認証結果を受けて，  - ネットワークへのアクセス制御を行う．Ciscoは有線/無線LANスイッチともにIEEE802.1Xに標準対応．
  - 認証サーバ(Authentication Server)
    - ユーザ認証を行うサーバのこと．IEEE802.1X/EAPに対応したRadiusサーバを使用する．
  - 認証に証明書を使う場合はCA(Certificate Authority)が必要．
  - 802.1Xの認証にはTACACS+は利用できない．radius only.

### EAP(PPP Extensible Authentication Protocol)
- PPPを拡張したプロトコル
- EAPOL(EAP over LAN)
- 認証装置がサプリカントと認証装置の間のEAPをIPパケットに変換して認証サーバに送る．

- 認証方式
  - TLS
  - PEAP

### config
```
# AAAの有効化（設定済みである場合，設定は不要）
(config)# aaa new-model
# RADIUSサーバの登録
(config)# radius server <config-name>
(config-radius-server)# address ipv4 <address> auth-port <number> acct-port <number>
(config-radius-server)# key <string>

# RADIUSサーバを認証サーバグループに登録
(config)# aaa group server radius <group-name>
(config-sg-server)# server name <config-name>

# IEEE802.1X - グローバルでの有効化
# AAAの有効化（設定済みである場合，設定は不要）
(config)# aaa new-model
# IEEE802.1X認証リストの設定
(config)# aaa authentication dot1x default group [radius | group-name]
# IEEE802.1X認証をグローバルで有効化
(config)# dot1x system-auth-control
# IEEE802.1X認可リストの設定
(config)# aaa authorization network default group [radius | group-name]

# IEEE802.1X - ポートでの有効化
# IEEE802.1X認証を有効化するI/Fの指定とアクセスポートの指定
(config)# interface <interface-id>
(config-if)# switchport mode access
# IEEE802.1X認証をポートで有効化
(config-if)# authentication port-control [ auto | force-authorized | force-unauthorized]
  - auto: 認証が成功すると許可ステート，失敗すると無許可ステート
  - force-authorized: 強制的に許可ステート
  - force-unauthorized: 強制的に無許可ステート

- 無許可ステート: EAPOL，CDP，STPトラフィック以外は送信されない
- 許可ステート: トラフィックが通常通り転送される

# IEEE802.1X port access entity (PAE) authenticatorとしてのみ動作する設定
(config-if)# dot1x pae authenticator
```
- `show authentication sessions interface <interface-id>`
  - 802.1X系のいろんな情報がみれる

## DHCP Spoofing/Snooping
### Spoofing対策
- DHCPスヌーピング: DHCPスプーフィング対策(DHCPクライアント，DHCPサーバのなりすまし対策)
- IPソースガード: IPアドレススプーフィング対策(送信元IPアドレスのなりすまし対策)
- Dynamic ARP Inspection: ARPスプーフィング対策(ARPリプライのなりすまし対策. DAIとも呼ばれる)

### DHCP Snooping
- DHCPクライアントのMACアドレス，IPアドレス，リースバインディングタイプ，VLAN，スイッチのポートからバインディングテーブルを構成
- Port
  - Trusted
    - DHCPサーバからのトラフィックを受信するポート
    - すべてのDHCPメッセージの送信元になれるポート
  - Untrusted
    - DHCPクライアントからのトラフィックを受信するポート
    - DHCPクライアントからのDHCP要求の送信元にのみなれるポート
- DHCP Option 82
  - DHCPスヌーピングだけでなくDHCP Option82が有効化されたスイッチでは，DHCPクライアントからのDHCP DiscoverメッセージをDHCPサーバに転送する前に「スイッチのMACアドレス，スイッチのポート番号」情報をパケットに追加するので，同じスイッチに接続している複数のLANホストを一意に識別可能
- config
```
# DHCPスヌーピングの有効化
(config)# ip dhcp snooping

# DHCPスヌーピングを有効にするVLANの指定
(config)# ip dhcp snooping vlan vlan-id [ smartlog]
#ドロップされたパケットの内容をNetFlowの収集装置に送信したい場合はsmartlogオプションを指定

# DHCP option 82フィールドの挿入・削除の有効化
(config)# ip dhcp snooping information option

# Trustポートの設定(defalut: untrust)
(config)# interface <interface-id>
(config-if)# ip dhcp snooping trust

# Options
# DHCPパケットの送信元MACアドレスのチェック
(config)# ip dhcp snooping verify mac-address
# Option82のデータ挿入で使用されるリモートIDの設定
(config)# ip dhcp snooping information option format remote-id [ string ASCII-string | hostname]
# DHCPスヌーピングデータベースエージェントの指定
(config)# ip dhcp snooping database <url>
```

- `show ip dhcp snooping`: DHCPスヌーピングが有効かどうか，有効化の対象VLANは何か，信頼ポートはどれかの確認
- `show ip dhcp snooping binding`: スイッチで学習された既知の「DHCPスヌーピングバインディングテーブル」の確認
- `show ip dhcp snooping statistics`: DHCPスヌーピングの統計情報を要約または詳細形式で確認
- `show ip dhcp snooping database`: DHCPスヌーピングバインディングテーブルのステータスと統計情報を確認

### IP ソースガード
- Source IP Spoofingを防止
- 送信元IPアドレス，MACアドレス，着信ポート番号のバインディングを追跡
- config
```
# DHCPスヌーピングの有効化
(config)# ip dhcp snooping

# DHCPスヌーピングを有効にするVLANの指定
(config)# ip dhcp snooping vlan vlan-id [ smartlog]
#ドロップされたパケットの内容をNetFlowの収集装置に送信したい場合はsmartlogオプションを指定

# DHCP option 82フィールドの挿入・削除の有効化
(config)# ip dhcp snooping information option

# 1 or 2でsouce guardを設定
# 1. IPソースガードの有効化（ 送信元IPアドレスによるフィルタリング ）
# 送信元IPアドレスがDHCPスヌーピングバインディングテーブルのエントリ，または，手動設定のバインディングテーブルのエントリに合致した場合に転送
(config-if)# ip verify source
# 3. IPソースガードの有効化（ 送信元IPアドレスと送信元MACアドレスによるフィルタリング ）
# 送信元IPアドレスだけでなく送信元MACアドレスの両方が合致した場合に転送
# DHCPサーバは「DHCPオプション82」をサポートしている必要がある
(config-if)# ip verify source port-security

# Options: スタティックに「IPソースバインディング」を追加する場合には以下のコマンドで定義します．
# DHCPスヌーピングを有効にするVLANの指定
(config)# ip source binding <mac-address> vlan <vlan-id> <ip-address> inteface <interface-id>

```

### Dynamic ARP Inspection(DAI)
- 環境とrequirement
  - DHCP環境
    - DHCP Snoopig有効化が必要
  - 非DHCP環境
    - DHCP Snooping有効化は不要

- 流れ
  1. Catalyst上の信頼できないポート上の全てのARP要求とARP応答を，Catalystスイッチで代行受信．
  1. 代行受信された各パケットに有効なIPアドレスとMACアドレスのバインディングがあるかを確認．
  1. Catalyst上のバインディングテーブル上に存在しない組み合わせである場合，その無効なARPを破棄．

- DHCP環境config
```
# DHCPスヌーピングの有効化
(config)# ip dhcp snooping

# DHCPスヌーピングを有効にするVLANの指定
(config)# ip dhcp snooping vlan vlan-id [ smartlog]
#ドロップされたパケットの内容をNetFlowの収集装置に送信したい場合はsmartlogオプションを指定

# DAIをVLAN単位で有効化
(config)# ip arp inspection vlan <vlan-id>

# optional: 現在ロギングされているどのパケットもスマートロギングされることを指定
(config)# ip arp inspection smartlog

# Trust interface の指定(defalut: untrust)
(config)# interface <interface-id>
(config-if)# ip arp inspection trust
```
- 非DHCP環境config
```
# ARP検査のためのARP ACLの定義
(config)# arp access-list <acl-name>
(config-arp-acl)# permit ip host <sender-ip> mac host <sender-mac> [ log]

# ARP ACLのVLANへの適用
(config)# ip arp inspection filter <acl-name> vlan <vlan-range> [ static]
- staticを指定するとARP ACLに合致しない場合にフレームを破棄．
- static指定しない場合，ARP ACLに合致しない場合DHCPスヌーピングバインディングテーブルを参照して判断

# Trust interface の指定(defalut: untrust)
(config)# interface <interface-id>
(config-if)# ip arp inspection trust
```

- rate-limit
  - arp inspectionする場合，untrustedポートでデフォルトで有効．DoS等の緩和のため．
```
# 着信ARPパケットのレート制限(defalut: 5pps, burst interval 1sec)
(config)# ip arp inspection limit [ rate pps [ burst interval seconds] | none]

# rate-limitに引っかかるとerr-disableになる．
# DAI の errdisableステートからのエラー回復をイネーブルにして，DAI の回復メカニズムで使用する変数を設定
(config)# errdisable detect cause arp-inspection
(config)# errdisable recovery cause arp-inspection
(config)# errdisable recovery interval <interval: default300sec>
```

- その他
```
# ARPパケットの妥当性のチェック
(config)# ip arp inspection validate [ src-mac] [ dst-mac] [ ip]
  - src-mac: イーサネットヘッダーの送信元MACアドレスと，ARPの送信元MACアドレスを比較
  - dst-mac: イーサネットヘッダーの宛先MACアドレスと，ARPの宛先 MAC アドレスが比較
  - ip: ARP本文から，無効なIPアドレスや予期しないIPアドレスがないかを確認してチェック

# ログバッファの設定
## レートとか
(config)# ip arp inspection log-buffer [ entries number | logs number interval seconds]
  - デフォルトで破棄パケットをログする．

# パケットのタイプを制御
(config)# ip arp inspection vlan vlan-id logging [ acl-match [ matchlog | none] | dhcp-bindings [ all | none | permit]
```

## NTP
- UDP: 123
- 最上位のNTPサーバ: stratum1, stratum15まで同期可能．
- 1900年1月1日0時0分0秒（UTC）を起点とする32ビットの積算秒数で表す
  - 2036年問題
- NTPv4だとv6も使える．セキュリティが向上．NTPv3と下位互換性あり．

- 同期モード
  - server/clientモード
    - NTPクライアントがNTPサーバに対して一方的に同期を行うモード
  - Symmetric, Active／Passiveモード
    - 同じ階層に属するNTPサーバ同士の場合に使われる
    - グループ化された同期メンバーの中から，基準となるNTPサーバを動的に決定する

- SNTP(Simple NTP)
  - クライアント専用.serverにはなれない.

- config
```
# タイムゾーンの設定
(config)# clock timezone JST 9
# システムクロックの設定(手動)
# clock set hh:mm:ss DATE MONTH YEAR

# ハードウェアクロックの設定
# calendar set hh:mm:ss DATE MONTH YEAR

# システムクロックの時刻情報をハードウェアクロックの時刻情報に同期
# clock update-calendar

(config)# ntp server [ ip-address | hostname] [prefer]
  - prefer: 優先して同期
# NTPサーバと時刻同期する際に，送信元アドレスを指定
(config)# ntp source <interface>

# NTPサーバ（マスタークロック）として設定
(config)# ntp master <stratum>

# Symmetric Active/Passiveモード
(config)# ntp peer address

# Broadcast/Multicast
(config-if)# ntp broadcast
(config-if)# ntp broadcast client
(config-if)# ntp multicast
(config-if)# ntp multicast client

# Authenticastion
# 1. NTP認証機能の有効化
(config)# ntp authenticate
# 2. NTP認証キーの定義
(config)# ntp authentication-key <number> md5 <value>
# 3. 2で定義したキーの番号を指定
(config)# ntp trusted-key <key-number>
# 4. 認証が必要なNTPサーバを指定（NTPクライアント側の設定）
(config)# ntp server <ip-address> key <key-number>

# NTPに対するACL制御
(config)# ntp access-group [ query-only | serve-only | serve | peer] <acl-number>

# 特定インターフェースでのNTPサービスの無効化
(config)# interface <interface-id>
(config-if)# ntp disable

# NTPのアソシエーションの最大数の制限
(config)# ntp max-associations <numbers>
```

## Syslog
- TDP/UDP: 514

- config
```
# バッファサイズ
(config)# logging buffered <size>

# syslog転送先の指定
(config)# logging host <ip-address>

# syslog転送するseverity
(config)# logging trap <level>s

# facility指定
(config)# logging facility <facility-type>

```
- ログで邪魔されたくない
```
(config)#line console 0
(config-line)#logging synchronous
(config)#line vty 0 15
(config-line)#logging synchronous
```

## SNMP(Simple Network Management Protocol)
- protocol
  - manager -> agent: UDP161
  - agent -> manager: UDP162

- versions
```
+----------+------------+-----------------------------------------------------------------+
| SNMP ver | RFC        | Descriptions                                                    |
+----------+------------+-----------------------------------------------------------------+
| SNMPv1   | RFC 1157   | SNMPコミュニティによる平文認証．SNMPトラップにおける再送確認なし        |
| SNMPv2c  | RFC 1901   | SNMPコミュニティによる平文認証．SNMPトラップにおける再送確認あり        |
| SNMPv3   | RFC 2273-5 | ユーザ単位の暗号化されたパスワード認証．SNMPトラップにおける再送確認あり  |
+----------+------------+-----------------------------------------------------------------+
```
  - v3ではセキュリティ等が強化
    - ユーザID/PWによる認証
      - コミュニティ名ではなくUSM（User-based Security-Model)でuser名/PWによる認証
    - VACM（View-based Access Control Model
      - ユーザごとに可能なMIBの範囲を定義できる昨日

- message
```
+-----------------+--------------+-------------------------------------------------------+
| Message         | Sender       | Descriptions                                          |
+-----------------+--------------+-------------------------------------------------------+
| Get Request     | SNMP Manager | SNMPエージェントから得たい情報を，OIDを指定して要求         |
| GetNext Request | SNMP Manager | 直前に指定したOIDの次のOIDを指定して要求．                 |
| Set Request     | SNMP Manager | SNMPエージェントの設定変更を行いたい場合，OIDを指定して要求  |
| Get Response    | SNMP Agent   | SNMPマネージャから要求されたOIDに対して，値を挿入して返信    |
| TRAP            | SNMP Agent   | SNMPエージェントが機器の状態に変化があった場合，自発的に送信  |
+-----------------+--------------+-------------------------------------------------------+
| GetBulk Request | SNMP Manager | 一括参照要求と言われるように，高速に複数の管理情報を取得      | # SNMPv2で追加
| Inform Request  | SNMP Agent   | SNMPエージェントからマネージャへ通知する時に確認応答も要求    | # SNMPv2で追加
+-----------------+--------------+-------------------------------------------------------+
```

- config
```
(config)# snmp-server community <community_name> [view <view-name>] [ro|rw] [acl_num]

(config)# snmp-server host <ip_addr> <community_name>
(config)# snmp-server host <ip_addr> version 2c <community_name>
(config)# snmp-server host address [ informs | traps] [ version 1 | 2c | 3 auth | noauth | priv] <community-string> [type]

(config)# snmp-server enable traps [notification-types]


# SNMPv3 - SNMPグループの設定 - ユーザ作成
(config)# snmp-server user username groupname [ remote address v3 [ encrypted]auth [ md5 | sha] auth-password [ priv [ des | 3des | aes [ 128 | 192 | 256] priv-password
```

- RMON(Remote Monitoring MIB)

## Netflow
- Key-Fields
  - 送信元IPアドレス
  - あて先IPアドレス
  - TCP/UDPポート送信元番号
  - TCP/UDPポートあて先番号
  - L3プロトコル
  - ToSバイト（DSCP）
  - 入力インターフェース
- Non Key-Fields
  - 送信元IPアドレス/あて先IPアドレス
  - 送信元ポート番号/あて先ポート番号
  - 入力インターフェースと出力インターフェース
  - ToSバイト（DSCP）
  - フローのバイト数，パケット数
  - 送信元AS番号，あて先AS番号

- Timer
  - Active timer(Defalut: 30min)
  - Inactive timer(Defalut: 15sec)

- config
```
# インターフェースへのNetFlowの適用
(config)# interface <interface-id>
(config-if)# ip flow [ ingress | egress]

(config)# ip flow-export version [ 1 | 5 | 9]　[ origin-as | peer-as] [ bgp-nexthop]
(config)# ip flow-export destination <address> <port>
(config)# ip flow-export source <interface>

# Options: NetFlowのキャッシュのチューニング
(config)# ip flow-cache entries number (default: 64536 entries)
(config)# ip flow-cache timeout active minutes (defalut: 30 min)
(config)# ip flow-cache timeout inactive second (default: 15sec)
```
- `show ip cache flow`
- `show ipv6 cache flow`

## PCAP



## Misc.
- ICMPリダイレクトの無効化
  - `(config-if)# no ip redirect`
  - たぶんL3インタフェースじゃないとだめ
- DSCP値