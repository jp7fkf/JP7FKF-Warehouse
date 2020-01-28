# Cisco - OSPF

## About ospf
- link-state type
- LSA(Link State Advertisement)をLSDB(Link Stete Database)に保存し，そのリンク情報をDijkstra Algorithmで計算し最短パスを計算．

- Packet
  - IP Protocol Number: 89
  - Hello: neighbor確立．キープアライブ． (multicast: 224.0.0.5)
    Helloパケットは，ネイバーを検出するためのパケット．ネイバーを検出してネイバー関係を確立した後の
    キープアライブ（ネイバー維持）としても使用される．マルチキャスト ( 224.0.0.5 ) として送信される．
  - DBD: DBD（Database Description）パケットは，自身のLSDBに含まれているLSAのリスト一覧．ネイバー
    ルータとこのDBDを交換し合うことにより，自身に不足しているLSAが何なのかを認識することができる．
  - LSR: LSR（Link State Request）パケットは自身のLSDBに不足しているLSAがあった場合，ネイバールータに
    その特定のLSAを要求するために使用される．
  - LSU: LSU（Link State Update）パケットは，LSRによりネイバーから要求されたLSAを送信するために使用．
    - LSAにはtype1-type11まで存在する．
  - LSAck: LSAck（Link State Acknowledgement）は，LSUを受信したことを通知するための確認応答として送信．

- Cost
  - default: cost = 100(Mbps) / Interface(Link) Bandwidth(Mbps)

- Router Type
  - Internal Router
  - Backbone Router
  - ABR(Area Boarder Router)
  - ASBR(AS Boundary Router)

- router-id
  1. router-id コマンドで設定した値
  1. ループバックインターフェースの中で最も大きいIPアドレス
  1. アクティブなインターフェースの中で最も大きいIPアドレス

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
  1. Type2: ネットワークLSA
    - 生成元: DR
    - 自エリアでのみフラッディング．
    - DRのIPアドレス，セグメント上のルータID一覧を含む
    - `show ip ospf database network`
    - code on routing table: `O`
  1. Type3: ネットワーク集約LSA
    - 生成元: ABR
    - 全エリアフラッディング(stub area, NSSAを除く)
    - 各エリアごとの経路情報，コスト値を含む(経路集約は手動で設定し通知する必要がある)
    - `show ip ospf database summary`
    - code on routing table: `O IA`
  1. Type4: ASBR集約LSA
    - 生成元: ABR
    - 全エリアフラッディング(stub area, NSSAを除く)
    - ABRが知っているASBRのルータID，コスト値を含む
    - `show ip ospf database asbr-summary`
    - code on routing table: `O E1 / O E2`
  1. Type5: AS外部LSA
    - 生成元: ASBR
    - 全エリアフラッディング(stub area, NSSAを除く)
    - 再配布した経路情報，コスト，ネクストホップを(外部AS(非OSPF)にある情報)含む
    - `show ip ospf database external`
    - code on routing table: `O E1 / O E2`
  1. Type7: NSSA外部LSA
    - 生成元: NSSAのASBR
    - NSSA
    - 再配布した経路情報，コスト，ネクストホップを(外部AS(非OSPF)にある情報)含む
    - `show ip ospf database nssa-external`
    - code on routing table: `O N1 / O N2`
  - `O`はOSPF, `E`はExternal, `N`はNSSA?

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

- Authentication
  - #TODO

## configuration
```
router ospf 1 # process id
  router-id 1.1.1.1

  # network address wildcard-mask area area-id
  network 172.16.1.0 0.0.0.255 area 0
  network 172.16.1.1 0.0.0.0 area 0

  maximum-paths 10 # set maximum ecmp path (default:4, max:16)

  passive-interface Loopback0 # set passive interface

  passive-interface default   # if you set default passive, use together with `no passive-interface` sentence to set active interface.
  no passive-interface GigabitEthernet 0/1

interface GigabitEthernet 0/1
  ip ospf priority 10 # ip ospf priority number for DR/BDR selection
  ip ospf cost 100    # set ospf cost
  auto-cost reference-bandwidth 1000  # set auto-cost numerator
  bandwidth 1000  # set bandwidth for cost calculation

  ip ospf network [broadcast|point-to-point|non-broadcast|point-to-multipoint|point-to-multipoint nonbroadcast]

  ip ospf hello-interval 10
  ip ospf dead-interval 40

  ip ospf dead-interval minimal hello-multiplier 5 # fast hello(5 hellos sent in 1sec. dead-interval is set 1sec automtically)

  # Route Addgegation
  summary-address 172.16.0.0 255.255.0.0 # at ASBR
  area 10 range 172.16.0.0 255.255.0.0 [ cost cost ] # at ABR

  # default route
  default-information originate [always] [metric metric] [metric-type [ 1 | 2 ]]

  # virtual link
  area 0 virtual-link 1.1.1.1 # area <area-id> virtual-link <router-id>
```

## commands
// ここはt-shootのために見直し，もっと書く．
- `show ip protocols`
- `show ip ospf`
- `show ip ospf interface`
- `show ip ospf neighbor`
- `show ip ospf database`
- `debug ip ospf events`
- `undebug [all|hoge]`
- `clear ip ospf proccess`
  - router-idを変更した場合など．ospfプロセス再起動が必要．

## debug/t-shoot

## References
