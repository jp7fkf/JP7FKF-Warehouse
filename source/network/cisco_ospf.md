# Cisco - OSPF

## About ospf
- link-state type
- LSA(Link State Advertisement)をLSDB(Link Stete Database)に保存し，そのリンク情報をDijkstra Algorithmで計算し最短パスを計算．

- Packet
  - IP Protocol Number: `89`
  - Hello: neighbor確立．キープアライブ． (multicast: 224.0.0.5)
    Helloパケットは，ネイバーを検出するためのパケット．ネイバーを検出してネイバー関係を確立した後の
    キープアライブ（ネイバー維持）としても使用される．マルチキャスト(224.0.0.5)として送信される．
  - DBD: DBD（Database Description）パケットは，自身のLSDBに含まれているLSAのリスト一覧．ネイバー
    ルータとこのDBDを交換し合うことにより，自身に不足しているLSAが何なのかを認識することができる．
  - LSR: LSR（Link State Request）パケットは自身のLSDBに不足しているLSAがあった場合，ネイバールータに
    その特定のLSAを要求するために使用される．
  - LSU: LSU（Link State Update）パケットは，LSRによりネイバーから要求されたLSAを送信するために使用．
    - LSAにはtype1-type11まで存在する．
  - LSAck: LSAck（Link State Acknowledgement）は，LSUを受信したことを通知するための確認応答として送信．

- Multicast
  - 全OSPFルータ宛: `224.0.0.5`
  - DR/BDR宛: `224.0.0.6`

- Cost
  - default: cost = 100(Mbps) / Interface(Link) Bandwidth(Mbps)

- Router Type
  - Internal Router
  - Backbone Router
  - ABR(Area Boarder Router)
  - ASBR(AS Boundary Router)

- router-id
  1. router-id コマンドで設定されたIPアドレス
  1. shutdown させていないループバックインターフェイスに設定された最も大きいIPアドレス
  1. shutdown させていない物理または論理インターフェイスに設定された最も大きいIPアドレス

- Neighbor
  - OSPF neighborを確立するためには以下の7項目をneighbor routerと完全に一致させる必要がある．
    1. Area-ID
    1. Authentication (if auth. enabled)
    1. Subnet mask
    1. Hello Interval
    1. Dead Interval
    1. Stub area flag (optional)
    1. Interface MTU size
  - State
    - Down: OSPFプロセス起動直後．リンクアップするとHelloを送出を開始．
    - Init: Helloを受け取った状態．
    - 2Way: Helloの送信と受信の両方が行われた状態．
  - 2Way stateになったのち，DR/BDR選出．
    1. OSPFプライオリティ値が最も大きいルータがDRとなる．
    1. OSPFプライオリティ値が2番目に大きいルータがBDRとなる．
    1. OSPFプライオリティ値が同じ場合，ルータIDが最も大きいルータがDR，2番目に大きいルータがBDR．

- Hello / Dead Interval
  - P2P, Broadcast: 10/40
  - P2MP, P2MP(Non-Broadcast), NBMA: 30/120

- neighbor確立条件
  1. area-id
  1. authentication
  1. network-mask
  1. Hello interval
  1. Dead interval
  1. stub flag
  1. MTU size
    - `(config-if)# ip ospf mtu-ignore`できる

- Adjacency
  - neighbor のうち，LSAを交換するもの(p2pならneighborと同値．Multi-access networkなら　DR, BDR)のことをいう．
  - neighborが確立されたあと．
  - State
    - Exstart: 起動後状態．ルータIDの大きい方からDBDパケットを送出する．Adjcencyになりうるルータのうち，ルータIDの大きい方をマスタ，小さい方をスレーブと呼ぶ．
    - Exchange: 双方のLSDBの交換状態．マスタからDBDを送出し，スレーブからも送信する．
    - Loading: DBDをもとに自身と相手のLSDBを比較し，不足しているlink state情報(LSA)についてLSRを送出しLSUを要求．
    - Full: 双方のLSDBが同期された状態．同期プロセスの完了．

- LSA Type
    1. Type1: ルータLSA
        - 生成元: OSPFルータ
        - 自エリアでのみフラッディング．
        - ルータID，リンク数，リンクの種類，コスト値を含む
        - `show ip ospf database router`
        - code on routing table: `O`
        - passive-interface はこっち．
    1. Type2: ネットワークLSA
        - 生成元: DR
        - 自エリアでのみフラッディング．
        - DRのIPアドレス，セグメント上のルータID一覧を含む
        - `show ip ospf database network`
        - code on routing table: `O`
        - EthernetのようなMultiaccess環境だと，多数のnodeが存在するときにLSA type1のみでは無駄なデータ量が増えるので，DRをrootとするLSA type2で必要な情報のみを伝搬する．
    1. Type3: ネットワーク集約LSA
        - 生成元: ABR
        - 全エリアフラッディング(stub area, NSSAを除く)
        - 各エリアごとの経路情報，コスト値を含む(経路集約は手動で設定し通知する必要がある)
        - `show ip ospf database summary`
        - デフォルトルートはこれ．
        - code on routing table: `O IA`
    1. Type4: ASBR集約LSA
        - 生成元: ABR
        - 全エリアフラッディング(stub area, NSSAを除く)
        - ABRが知っているASBRのルータID，コスト値を含む
        - ASBRに到達するためのメトリックのこと．
        - `show ip ospf database asbr-summary`
        - code on routing table: `O IA`
    1. Type5: AS外部LSA
        - 生成元: ASBR
        - 全エリアフラッディング(stub area, NSSAを除く)
        - 再配布した経路情報，コスト，ネクストホップを(外部AS(非OSPF)にある情報)含む
        - `show ip ospf database external`
        - code on routing table: `O E1 / O E2`
        - 初期でE, type2．コスト加算されない．
        - redistributeはこれ．
    1. Type7: NSSA外部LSA
        - 生成元: NSSAのASBR
        - NSSA内部only
        - 再配布した経路情報，コスト，ネクストホップを(外部AS(非OSPF)にある情報)含む
        - `show ip ospf database nssa-external`
        - code on routing table: `O N1 / O N2`
        - 初期でE, type2．ASBRまでのコスト加算がされない．
    - `O`はOSPF, `E`はExternal, `N`はNSSA?
    - ちなみに`E1/E2`, `N1/N2`の違いは下記
      - `1`: コスト加算型(O E1)
        - シードメトリック値に経由するルータのコスト値が加算されていく
      - `2`: コスト透過型(O E2)
        - シードメトリック値のまま変化せず、OSPFネットワークへ通知されていく
    - LSAのSeq. Noは0x80000001から始まり，Updateするごとに1ずつインクリメントする．
    - LSAにはaging timerが存在し，60分を過ぎた場合は該当のLSAを削除する．

- エリア
  - Backbone Area
    - area 0
    - LSA: 1-5
  - Standard Area
    - LSA: 1-5
    - バックボーン，Stub, NSSAでないエリア．
  - Stub Area
    - LSA: 1-3
    - 非OSPFネットワークとは，ASBRのルート(LSA type5)ではなく，ABRの配布するデフォルトルートを用いてアクセスする
      - デフォルトルートは自動生成される
    - ASBRの配置不可．仮想リンク設定不可．
    - ABRは自動的にdefault routeを生成する．
    - エリア内全ルータで`area <area-id> stub`
  - Totally Stub Area
    - LSA: 1-2
    - Stub Area同様，ASBRのルート(LSA type5)ではなく，ABRの配布するデフォルトルートを用いて非OSPFネットワークとアクセスする
      - デフォルトルートは自動生成される
    - さらにABRのLSA type3のルートも利用せず，ABRが広告するデフォルトルートのみを利用する．
    - つまり非OSPFネットワークおよび他エリアネットワークの両方をデフォルトルートを用いてアクセスする．
    - エリア内全ルータで`area <area-id> stub`，ABRのみ`area <area-id> stub no-summary`
  - Not so Stubby Area(NSSA)
    - LSA: 1-3, 7
    - Stub AreaにASBRが存在．
    - NSSAのASBRから外部ルートが再配布された場合LSA type7により伝搬(NSSA内only)
    - NSSAから外部areaに向けてはABRでLSA type5として伝搬
    - 多エリアからの外部経路はデフォルトルートを用いるが，NSSAのABRではデフォルトルートを手動で生成する必要がある．
    - エリア内全ルータにて`area <area-id> nssa`
    - ABRにて`area <area-id> nssa default-information-originate`
      - デフォルトルートは自動生成されない
  - Totally Not so Stubby Area(Totally NSSA)
    - LSA: 1-2, 7
    - デフォルトルートは自動生成される
    - エリア内全ルータにて`area <area-id> nssa`
    - ABRにて`area <area-id> nssa no-summary`

- Network Type
  - P2P(Point-to-Point)
    - DR/BDR選出は行われず必ずadjecencyとなる．
  - Broadcast Multi Access
    - DR/BDR選出が行われる．
  - NBMA(Non-Broadcast Multi Access)
    - 5つのmodeがある
      - NBMA
      - Broadcast
      - Point-to-Point
      - Point-to-Multipoint
      - Point-to-Multipoint(Non-Broadcast)
    ```
    +---------------------+----------+-----------+--------------------+---------------------+----------+
    |      NBMA Mode      | Neighbor |  DR/BDR   |    Appropreate     |   ip ospf network   |  Hello   |
    |                     |  Detect  | Candidate |      Topology      |                     | Interval |
    +---------------------+----------+-----------+--------------------+---------------------+----------+
    | NBMA                |  Manual  |    Yes    |     Full Mesh      |    non-broadcast    |    30    |
    | Broadcast           |   Auto   |    Yes    |     Full Mesh      |      broadcast      |    10    |
    | Point-to-Point      |   Auto   |    No     | Star(subInterface) |    point-to-point   |    10    |
    | Point-to-Multipint  |   Auto   |    No     |        Star        | point-to-multipoint |    30    |
    | Point-to-Multipoint |  Manual  |    No     |        Star        | point-to-multipoint |    30    |
    |   (Non Broadcast)   |          |           |                    |    nonbroadcast     |          |
    +---------------------+----------+-----------+--------------------+---------------------+----------+
    ```
- Virtual Link
  - 全てのareaは必ずbackbone area(Area 0)に接続している必要があるが，直接接続できない場合，virtual linkで論理的に接続することが可能．
  - `(config-router)# area <transit-area-id> virtual-link <neighbor-router-id>`

- Authentication
  - インターフェイス認証
  ```
  (config-if)# ip ospf authentication [message-digest] #md5の場合のみmessage-digest.平文の場合は不要．
  (config-if)# ip ospf message-digest-key <key-id> md5 <key> #md5の場合
  (config-if)# ip ospf authkentication-key <plaintext-password> #平文の場合
  ```
  - エリア認証
  ```
  (config-router)# area <area-id> authentication [message-digest] #md5の場合のみmessage-digest.平文の場合は不要
  # 認証password/digestは各interface にて設定する．
  (config-if)# ip ospf message-digest-key <key-id> md5 <key> #md5の場合
  (config-if)# ip ospf authkentication-key <plaintext-password> #平文の場合
  ```
  - virtual link認証
  ```
  (config-router)# area <area-id> virtual-link <router-id> message-digest-key <key-id> md5 <key> #md5の場合
  (config-router)# area <area-id> virtual-link <router-id> authkentication-key <plaintext-password> #平文の場合
  ```

- LSA pacing
  - OSPFは、lsa-group pacingと呼ばれる一定間隔のLSA処理がある．
  - 複数のLSAを1つのグループでまとめて管理することでリソースの効率化を図る．
  - checksumによるLSA破損有無の確認を行う．(defalutで10分ごと)
  - aging time30分を過ぎていた場合はそのLSAを再送する
  - aging time60分を過ぎていた場合はそのLSAを破棄する．

  - defalutでlsa-group pacingは240秒間隔で行われる．
    - 30min 経過したLSAは発信元ルータからLSAが再度送信されてRefreshされる．
    - 長いほど一度に送られるLSUが多くなるのでパラメータ調整が可能．短くすると一度に該当するLSAが減るので量が減る．
    ```
    (config)#router ospf <proccess-id>
    (config-router)#timers pacing lsa-group <sec>
    ```
    - `show ip ospf timers lsa-group`

- Redistribution
  - シードメトリックは20
    - ただしBGPからの再配布の場合は1
  - distribution-list の扱い
    - distribute-list out
      - distribute-list outコマンドは再配送されたOSPF LSA Type-5に対してのみ有効となる
      - redistributeした上で，distribute-list outコマンドを利用することで特定の経路をフィルタできる．

  - distribute-list in
    - OSPFで学習した経路をルーティングテーブルに反映させたくない場合に利用できる．
    - LSA自体のfilteringは行われないためLSDBは通常通り構築される．
    - LSAのフラッディングも同様に行われるため，あくまでルーティングテーブルへの反映をしないとだけ思うべき．

- その他
  - Ciscoの推奨
    - 一台のルータは最大3areaまでの所属
    - 1areaあたり50台までのルータで構成

## configuration
```
router ospf 1 # process id
  router-id 1.1.1.1

  # network address wildcard-mask area area-id
  network 172.16.1.0 0.0.0.255 area 0
  network 172.16.1.1 0.0.0.0 area 0

  area <area_id> [stub|stub no-summary|nssa|nssa no-summary] # no-summaryはABRにのみ入れる．

  maximum-paths 10 # set maximum ecmp path (default:4, max:16)

  passive-interface Loopback0 # set passive interface

  passive-interface default   # if you set default passive, use together with `no passive-interface` sentence to set active interface.
  no passive-interface GigabitEthernet 0/1

  neighbor 192.168.0.2 # if it was NBMA network. it's can't accept ospf multicast, so neighbor address must be configured manually.

  refistribute eigrp 10 metric 100 subnets
  distribute-list 10 out eigrp

  # Route Addgegation
  summary-address 172.16.0.0 255.255.0.0 # at ASBR LSA type5
  area 10 range 172.16.0.0 255.255.0.0 [ cost cost ] # at ABR LSA type3

  # virtual link
  area 0 virtual-link 1.1.1.1 # area <area-id> virtual-link <router-id>

  # default route
  default-information originate [always] [metric metric] [metric-type [ 1 | 2 ]]

  # Authentication
  area area-id authentication [ message-digest(in MD5)]

interface GigabitEthernet 0/1
  auto-cost reference-bandwidth 1000  # set auto-cost numerator
  bandwidth 1000  # set bandwidth for cost calculation

  ip ospf priority 10 # ip ospf priority number for DR/BDR selection
  ip ospf cost 100    # set ospf cost
  ip ospf network [broadcast|point-to-point|non-broadcast|point-to-multipoint|point-to-multipoint nonbroadcast]
  ip ospf hello-interval 10
  ip ospf dead-interval 40

  ip ospf dead-interval minimal hello-multiplier 5 # fast hello(5 hellos sent in 1sec. dead-interval is set 1sec automtically)

  # Authentication
  ip ospf authentication-key <password> # plaintext
  ip ospf message-digest-key <key-id> md5 <password> # md5
```

## commands
// ここはt-shootのために見直し，もっと書く．
- `show ip protocols`
- `show ip ospf`
- `show ip ospf interface`
- `show ip ospf neighbor`
- `show ip ospf database`
  - LSAのSeq. Noは0x80000001から始まり，Updateするごとに1ずつインクリメントする．
- `debug ip ospf events`
- `undebug [all|hoge]`
- `clear ip ospf proccess`
  - `clear ip ospf [pid] {process | redistribution | counters [neighbor [neighbor-interface] [neighbor-id]]}`
  - Syntax Description
    - pid: (Optional) Process ID.
    - process: Reset OSPF process.
    - redistribution: Clear OSPF route redistribution.
    - counters: OSPF counters.
    - neighbor: (Optional) Neighbor statistics per interface.
    - neighbor-interface: (Optional) Neighbor interface.
    - neighbor-id: (Optional) Neighbor ID.
  - router-idを変更した場合など．ospfプロセス再起動が必要．

## debug/t-shoot

## References
- [OSPF - Type 1 LSA vs Type 5 LSA (passive vs redistribute) - Darren's Blog](https://mellowd.co.uk/ccie/ospf-type-1-lsa-vs-type-5-lsa/)
