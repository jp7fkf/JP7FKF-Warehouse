# Cisco - EIGRP

## About EIGRP
- Extended Distance Vector type
  - Hybrid type(Link-state and Distance Vector)

- Packet
  - Hello
    - neighbor確立，またneighbor のkeepaliveのために利用．multicast(224.0.0.10).
  - Update
    - routing update. 新規ルート検出時，収束完了時はmulticast(224.0.0.10)．ERGRP開始時の同期はunicast.
  - Query
    - route informationを要求する．
  - Reply
    - Queryのための応答．unicast.
  - Ack

- Table
  - neighbor table: 隣接関係を保持．Hello packetで作れる．
  - topology table: EIGRPで受け取った全ての経路情報を保持．Update packetで作れる．
  - routering table(RIB)

- Metric
  - 複合メトリック
    - K1(defalut:1): 帯域幅 (bandwidth): 10000 ÷ 最小帯域幅(Mbps) × 256
    - K2(defalut:0): 負荷 (load)
    - K3(defalut:1): 遅延 (delay): 遅延(10usec)の合計値 × 256
    - K4(defalut:0): 信頼性 (reliability)
    - K5(defalut:0): MTU
  - それぞれの値は`show interfaces`で確認できる．
  - 通常，`delay`の値を変更することでほとんどの要件は満たすことができる．
  - K値は，EIGRPを構成する隣接ネイバで同じ値とする(これが崩れるとパス選択が狂う)
  - K値は0-255で指定．

- 帯域幅と遅延値(参考)
  - Ethernet        :BW   10000 Kbit, DLY 1000 usec
  - FastEthernet    :BW  100000 Kbit, DLY  100 usec
  - GigabitEthernet :BW 1000000 Kbit, DLY   10 usec

- 経路計算
  - アルゴリズム: DUAL(Diffusing Update Algorithm)
  - ホップ数はdefaultで100．最大255．

  - 用語
    - S(Successor): プライマリールート．ルーティングテーブルに格納．最小のFDの経路．
    - F(Feasible Successor): バックアップルート．サクセサのダウン時に即サクセサとなるルート．
      - フィージブルサクセサは，FD>ADとなる経路．ループフリーを実現するため，FD>ADな経路はフィージブルサクセサにできない．
    - AD(Advertised Distance): ネイバールータから宛先ネットワークまでのメトリック値．
    - FD(Feasible Distance): ローカルルータから宛先ネットワークまでのメトリック値．

  - 等コストバランシング(ECMP, equal-cost load balancing)
    - 同一メトリックのFはdefault最大4つまでrouteing tableに格納される．最大値は16まで変更可能．
  - 不当コストバランシング(UCMP, unequal-cost load balancing)
    - 最小メトリクスのルートに対して何倍のメトリクスまでを不当コストバランシング対象とするか．
    - サクセサ or フィージブルサクセサのみが対象．
    - SのFDが100のとき，`varience 2`とすると200までのFDをもつパスが対象となる．

- その他
  - デフォルトルートの配布
    - staticで書いてredistribute
    - staticで書いて`network 0.0.0.0/0`
    - `ip default-network`でデフォルトルートを通知
      - クラスフルに指定する．
      - ex.) `ip default-network 192.168.0.0`
    - `ip summary-address eigrp`
      - `(config-int)#  ip summary-address eigrp 65000 0.0.0.0 0.0.0.0`
      - デフォルトルート以外は通知されなくなる．
      - 他で受け取ったdefault routeがnull0経路で上書きされることがある(AD値による)ので注意が必要．
  - offset list
    - metricをダイレクトに加算する．
  - stub設定
    - スタブ側のrouterで設定することで無駄なクエリを抑制できる．
    - optionで送信したい経路を選べる(connected, static, aggregated, redistributed)

## configuration
```
router eigrp 65000 #autonomous num(No relation between BGP AS num)
  network 172.16.1.0 0.0.0.255 # network addr/wildcard mask which is matched networks/interfaces you want to enable.
  network 172.16.1.1 0.0.0.0   # /32

  maximum-paths 16  # if you want to expand maximum-path value. (default: 4, 1-16)
  variance 2        # if you want to enable unequal-cost load balancing. (default:1, 1-128)

  (no)auto-summary  # if you want to (not) use auto summary(Classful). (Since IOS 15.x, default: diable)

  passive-interface GigabitEthernet0/1  # if you want to work interface as passive mode.

  passive-interface default   # default passive(all interfaces). if you activate interface, use together with `no passive-interfaces <int>`.

  distribute-list 1 out GigabitEthernet0/0

  neighbor 172.16.1.1 GigabitEthernet0/0  # unicast only(don't use multicast). static neighbor.

  offset-list 1 in 1000 GigabitEthernet0/0 # offset list (offset-list acl-number [in|out] offset interface-id)

  eigrp stub  # (eigrp stub [receive-only|connected|static|summary|redistributed])

interface GigabitEthernet0/0
  bandwidth 128 # kbps(optional)
  ip hello-interval eigrp 65000 30  #1-65535. set save value with neighbor
  ip summary-address eigrp 1 10.10.0.0 255.255.0.0  # manual route aggregate (set under "output" interface of aggregated route-updates)
  # if the manual aggregation enabled, the Null0 route of aggregated prefix will be installed in order to prevent routing loop.


access-list 1 deny any any    # for distribute-list
```
- 名前付きの場合(address-family)の一例
```
　Cisco(config) # router eigrp N-EIGRP
　Cisco(config-router) # address-family ipv4 unicast autonomous-system 10
　Cisco(config-router-af) # af-interface default
　Cisco(config-router-af-interface) # passive-interface

　Cisco(config-router-af) # af-interface Gigabitethernet0/1
　Cisco(config-router-af-interface) # no passive-interface
```

## commands
// ここはt-shootのために見直し，もっと書く．

- `show ip protocols`
  - EIGRP AS num， K値，max hop count，
- `shpw ip eigrp interfaces`
- `show ip eigrp traffic`
- `show ip eigrp neighbors`
- `show ip eigrp topology`
  - 通常サクセサとフィージブルサクセサの情報のみが出る．
  - 全ルート情報を見たい場合は`show ip eigrp topology all-links`
  -
  ```
  P(Passive)： ルート情報がEIGRPの計算中ではないことを示すコード．
  A(Active)： ルート情報がEIGRPの計算中であることを示すコード．
  U(Update)： Updateパケットがこの宛先へ送信されたことを示すコード．
  Q(Query)： Queryパケットがこの宛先へ送信されたことを示すコード．
  R(Reply)： Replyパケットがこの宛先へ送信されたことを示すコード．
  ```
- `debug ip eigrp`

## debug/t-shoot

## References
- [EIGRPをはじめから技術解説](https://www.infraexpert.com/study/study31.html)
