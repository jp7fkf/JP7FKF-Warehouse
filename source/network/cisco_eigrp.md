# Cisco - EIGRP

## About EIGRP
- Extended Distance Vector type
  - Hybrid type(Link-state and Distance Vector)

- AD値
  - EIGRP集約：5
  - 内部EIGRP：90
  - 外部EIGRP：170　

- Packet
  - Hello
    - neighbor確立，またneighbor のkeepaliveのために利用．multicast(224.0.0.10).
  - Update
    - routing update. 新規ルート検出時，収束完了時はmulticast(224.0.0.10)．ERGRP開始時の同期はunicast.
  - Query
    - route informationを要求する．
    - FSが存在しないときに代替経路を探索．
  - Reply
    - Queryのための応答．unicast.
  - Ack

  - このうちAckが必要(RCP)な高信頼パケットはUpdate, Query, Reply．Ackがない場合，16回まで再送信する．
  - Ackが必要ない無信頼パケットはHello, Ack

  - SRTT(SmoothRound-TripTimer)
    - 高信頼性パケット送信 →ACK受信確認までの時間

  - RTO(RetransmissionTimeout)
    - 高信頼性パケット送信→×→再送
    - この再送までの待ち時間のこと
      - この値はSRTT等から動的に決定される．
    - 最大パケット再送数は16回(1回目はMulticast, 2回目以降はUnicast)

  - RTP(ReliableTransportProtocol)
    - パケットが順番に運ばれることを保証

- Table
  - neighbor table: 隣接関係を保持．Hello packetで作れる．
  - topology table: EIGRPで受け取った全ての経路情報を保持．Update packetで作れる．
  - routering table(RIB)

- Hello interval/Holdtime
```
+-------------------+-----------------------------------+---------------+----------+
| Bandwidth         | Link Examples                     | HelloInterval | Holdtime |
+-------------------+-----------------------------------+---------------+----------+
| 1.544Mbps or less | Multipoint Framerelay             |        60 sec |  180 sec |
| over 1.544Mbps    | Ethernet, ISDN, BRI, PPP, or HDLC |         5 sec |   15 sec |
+-------------------+-----------------------------------+---------------+----------+
```
  - Hello interval, Holdtimeの変更はinterface configで
    - `(config-if)# ip hello-interval eigrp 65000 30  #1-65535. set save value with neighbor`
    - `(config-if)# ip hold-time eigrp 65000 30  #1-65535. set save value with neighbor`

- Metric
  - 複合メトリック
    - K1(defalut:1): 帯域幅 (bandwidth): 10^7 / 最小帯域幅(kbps)
      - 宛先までの最小帯域幅
    - K2(defalut:0): 負荷 (load)
      - 経路の実行帯域幅．100%なら255
    - K3(defalut:1): 遅延 (delay): 総遅延(us) / 10(us)
      - 宛先までの遅延値の合計．
    - K4(defalut:0): 信頼性 (reliability)
      - 0-255
    - K5(defalut:0): MTU
      - パスの最小MTU
  - これらの値を足した値に256をかけた値がメトリックとなる．2bit shift.
  - それぞれの値は`show interfaces`で確認できる．
  - 通常，`delay`の値を変更することでほとんどの要件は満たすことができる．
  - K値は，EIGRPを構成する隣接ネイバで同じ値とする(これが崩れるとパス選択が狂う)
  - K値は0-255で指定．

- 帯域幅と遅延値(参考)
  - Ethernet        :BW   10000 Kbit, DLY 1000 usec
  - FastEthernet    :BW  100000 Kbit, DLY  100 usec
  - GigabitEthernet :BW 1000000 Kbit, DLY   10 usec

- 経路計算
  - アルゴリズム: DUAL(Diffusing Update ALgorithm)
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

  - サクセサdown時のふるまい
    - サクセサ（Passive状態）Down→バックアップ（FS）なし（Active状態）→クエリ送信（Active状態：3分間）＝SIA（StuckInActive）⇒経路削除
      - ※クエリ送信時は収束（コンバージェンス）しない⇒SIAは経路に悪影響
    - SIA(Stack in Active)
      - クエリを送信して3分以上Ackがないこと．
      - 原因
        - ルータがBusy, Memory, CPUのリソース問題
        - 片方向のリンク障害
        - パケロス
        - Ackが届かないなんらかの原因．

- neighbor確立
  - EIGRP neighbor の確立条件
    - AS num
    - K値
    - Authentication
    - netmask
  - multicast IPは`224.0.0.10`を使用

- Authentication
  - MD5のみをサポート.
  - key idとkeyの両方がmatchする必要がある(双方を用いてhashをとるため)
  - keychain
    ```
    (config)# key chain <name>
    (config-keychain)# key <key-id>
    (config-keychain-key)# key-string <password>

    # optional(key-lifetime)
    (config-keychain-key)# accept-lifetime start-time { end-time | infinite | duration seconds }
    (config-keychain-key)# send-lifetime start-time { end-time | infinite | duration seconds }

    # apply to interface
    (config-if)# ip authentication mode eigrp <as-number> md5
    (config-if)# ip authentication key-chain eigrp <as-number> <keychain-name>
    ```
    - `show key chain` でkeychainを確認可．
    - `show ip eigrp neighbors`でneithborが張れていればok．
    - trouble shoot的には`debug eigrp packets`かな．


- スタブ
  - スタブが設定されたルータは自身がスタブであることを示すpacketを全neighborに送る．
  - これにより，neighborはスタブルータにQueryを送らなくなる．
  - スタブルータはdefaultでsummary経路とconnected経路をadvertise.

- 経路集約
  - 集約経路はIOS15.0以降は自動生成されない(no auto-summary)ので，明示的に設定する必要がある．
  - 集約経路のAD値はデフォルトで`5`．
  - 自動集約
    - `(config-router)# auto-summary`
    - クラスフルネットワーク境界で自動生成
  - 手動集約
    - 集約経路の一部がrouting tableにあることが前提．
    - `(config)# interface <interface-id>`
    - `(config-if)# ip summary-address eigrp <as-number> <address> <netmask> [AD]`

- ロードバランシング
  - 等コストバランシング
  - 不等コストバランシング
    - SとFSが対象．

- Redistribution
  - シードメトリックは無限大
    - メトリックを明示的に指定しなければ再配布できない．

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
  - EIGRPはIGRPのメトリック(24bit)を8bit拡張して32bitにしている(なので，x256, 2^8する必要がある)．そのためIGRPへの下位互換性がある．
  - HoldtimerはHello以外のパケットでもResetされる．
  - EIGRPはinterfaceに設定した帯域幅の50%までをEIGRPの帯域としてdefaultで利用する．(TODO:)
    - interfaceで`ip bandwidth-percent eigrp <as-num> <%>`で変更可．(100%を超えることも可能．メトリクス調整のため実効帯域よりも小さい値をbandwidthとして指定する可能性があるため．)

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
  ip hold-time eigrp 65000 30  #1-65535. set save value with neighbor

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
- [EIGRPをはじめから技術解説](https://www.infraexpert.com/study/study31.html): 3960
