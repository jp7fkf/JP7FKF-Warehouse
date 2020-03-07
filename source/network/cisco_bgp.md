# Cisco - BGP

## About BGP
- TCP: `179`
- Classless
- triggerd update(differenece update)

- Message
  - OPEN: TCPコネクション確立後にBGP neighborとのセッションを開始するためのメッセージ．BGP protocol version, AS-num, Router-ID, holdtime, authenticationの情報が含まれる．
  - UPDATE: ルート情報が含まれるメッセージ
  - KEEPALIVE: BGP neighborを維持するための生存メッセージ．defaultで60sec(Cisco)送信され，holddown-timerは180sec(Cisco)
  - NOTIFICATION: error検出時に通知されるメッセージ．es.)hostdown time超過.bgpセッションを切断する場合等．

- State
  - Idle
    - neighborへの疎通性があればTCP接続を開始する状態．
  - Connect
    - TCP接続の完了を待っている状態．TCP接続が完了した場合Openメッセージを送信してOpen Sentに遷移．TCP接続が失敗するとActiveへ遷移．
  - Active
    - TCP接続を施行している状態．
  - Open Sent
    - Openメッセージを送出し，neighborからのOPENメッセージを待機している状態．
    - neighborからのOPENが正しく受理された場合，OPEN Confirmへ遷移．
  - Open Confirm
    - KEEPALIVE/NOTIFICATIONを待っている状態．
    - KEEPALIVEを受信した場合Establishedに遷移．
    - NOTIFICATIONを受信した場合はIdleへ遷移．
  - Established
    - BGP neighborが確立している状態．
    - UPDATEメッセージ交換等を実施．
    - NOTIFICATIONメッセージを受信した場合はIdleへ遷移．

- table
  - neighbor table
    - `show ip bgp summary`
    - neighbor state: `show bgp neighbors`
  - bgp table
    - `show ip bgp`
  - routing table
    - `show ip route`

- check point: BGP neighbor UP
  - AS number
  - IP Reachability to neighbor IP
    - interface, TTLに注意．
  - Authentication
    - password, etc...

- Confederation
```
(config)# router bgp <as-number1> # コンフェデレーションにより分割された自分のAS番号
(config-router)# bgp confederation identifier <as-number2> # これが本来のAS番号(Global AS)
(config-router)# bgp confederation peers <as-number3> # コンフェデレーションによりピアする内部ASのAS番号
```

## configuration
```
(config)# router bgp as-number
(config-router)# neighbor <ip-address> remote-as <as-number>
# (config-router)#neighbor 10.2.2.2 update-source loopback 0
# (config-router)#neighbor 10.2.2.2 ebgp-multihop 2

(config-router)# network <network> mask <mask> # advertise network

(config-router)# timers bgp <keepalive_interval> <holdtime>

(config-route-map)# aggregate-address <address> <mask> [as-set] [as-confed-set] [summary-only] [advertise-map name] [attribute-map name] [supress-map name]

# peer-groupを使う場合
(config-router)# neighbor <group_name> peer-group
(config-router)# neighbor <peer_ip> peer-group <group_name>
(config-router)# neighbor <group_name> remote-as <as_num>

# outのポリシはpeer-groupを使う場合neighbor個別にはできない．グループ全体で一つ．
(config-router)# neighbor <group_name> route-map <map_name> out

# inboundポリシは個別設定できる．
(config-router)# neighbor <peer_ip> route-map <map_name> in


```

- RFC1771では、BGPのkeepalive intervalはholdtimeの1/3の値が推奨されている．

- clear bgp
  - BGPではメトリックの変更(ex. LOCAL_PREF, MED)ではネットワークの変更とみなされないため，UPDATEは即座に送出されない．
  - Peerを切らずにUPDATEを送出する場合: `clear ip bgp <neighbor ip-address> soft out`
  - Peerを切らずにルート情報の再送信を要求する
    - `clear ip bgp <neighbor-ip-address> soft in`
        - `neighbor <ip-address> soft-reconfiguration inbound`をいれておくとbgp filterを変えた時等にルート情報を再要求しなくてよいので便利
  - すべてのBGPピアをhard reset
    - `clear ip bgp *`

## commands
- `show ip route`

## debug/t-shoot

## References
